---
layout: default
title: Chương 5
parent: Đối tượng & Lớp
nav_order: 6
---

# You Don't Know JS Yet: Đối tượng & Lớp - Ấn bản thứ 2
# Chương 5: Ủy Quyền (Delegation)

| LƯU Ý: |
| :--- |
| Đang trong quá trình thực hiện |

Chúng ta đã khám phá kỹ lưỡng các đối tượng, nguyên mẫu, lớp, và bây giờ là từ khóa `this`. Nhưng bây giờ chúng ta sẽ xem xét lại những gì chúng ta đã học được cho đến nay từ một góc nhìn hơi khác.

Điều gì sẽ xảy ra nếu bạn có thể tận dụng tất cả sức mạnh của các cơ chế đối tượng, nguyên mẫu và `this` động cùng nhau, mà không bao giờ sử dụng `class` hoặc bất kỳ hậu duệ nào của nó?

Trên thực tế, tôi sẽ lập luận rằng JS vốn dĩ ít định hướng lớp hơn so với những gì từ khóa `class` có thể thể hiện. Bởi vì JS là một ngôn ngữ nguyên mẫu, động, thế mạnh của nó thực sự là... *ủy quyền* (delegation).

## Lời Mở Đầu (Preamble)

Trước khi chúng ta bắt đầu xem xét ủy quyền, tôi muốn đưa ra một lời cảnh báo. Góc nhìn này về các cơ chế `[[Prototype]]` của đối tượng và ngữ cảnh hàm `this` của JS *không phải* là chính thống. Đó *không phải* là cách các tác giả khung và thư viện sử dụng JS. Theo hiểu biết của tôi, bạn sẽ không tìm thấy bất kỳ ứng dụng lớn nào ngoài kia sử dụng mẫu này.

Vậy tại sao tôi lại dành một chương cho một mẫu như vậy, nếu nó không phổ biến đến thế?

Câu hỏi hay. Câu trả lời táo tợn là: bởi vì đó là cuốn sách của tôi và tôi có thể làm những gì tôi thích!

Nhưng câu trả lời sâu sắc hơn là, bởi vì tôi nghĩ rằng việc phát triển sự hiểu biết *này* về một trong những trụ cột cốt lõi của ngôn ngữ sẽ giúp bạn *ngay cả khi* tất cả những gì bạn từng làm là sử dụng các mẫu JS kiểu `class`.

Để rõ ràng, ủy quyền không phải là phát minh của tôi. Nó đã tồn tại như một mẫu thiết kế trong nhiều thập kỷ. Và trong một thời gian dài, các nhà phát triển lập luận rằng ủy quyền nguyên mẫu *chỉ* là dạng thừa kế động.[^TreatyOfOrlando] Nhưng tôi nghĩ rằng đó là một sai lầm khi gộp hai thứ đó lại với nhau.[^ClassVsPrototype]

Đối với mục đích của chương này, tôi sẽ trình bày ủy quyền, như được triển khai thông qua cơ học JS, như một mẫu thiết kế thay thế, được định vị ở đâu đó giữa định hướng lớp và các mẫu đối tượng-đóng/mô-đun.

Bước đầu tiên là *giải cấu trúc* (de-construct) cơ chế `class` xuống các phần riêng lẻ của nó. Sau đó, chúng ta sẽ chọn lọc và trộn các phần hơi khác một chút.

## Dù Sao Thì Constructor Là Gì? (What's A Constructor, Anyway?)

Trong Chương 3, chúng ta đã thấy `constructor(..)` là điểm nhập chính cho việc xây dựng một thể hiện `class`. Nhưng `constructor(..)` thực sự không thực hiện bất kỳ công việc *tạo* nào, nó chỉ là công việc *khởi tạo*. Nói cách khác, thể hiện đã được tạo vào thời điểm `constructor(..)` chạy và khởi tạo nó -- ví dụ: các kiểu gán `this.whatever`.

Vậy công việc *tạo* thực sự diễn ra ở đâu? Trong toán tử `new`. Như phần "Gọi Ngữ cảnh Mới" trong Chương 4 giải thích, có bốn bước mà từ khóa `new` thực hiện; bước đầu tiên trong số đó là tạo một đối tượng rỗng mới (thể hiện). `constructor(..)` thậm chí không được gọi cho đến bước 3 trong các nỗ lực của `new`.

Nhưng `new` không phải là cách duy nhất -- hoặc thậm chí có lẽ là tốt nhất -- để *tạo* một "thể hiện" đối tượng. Hãy xem xét:

```js
// một "constructor" không phải lớp
function Point2d(x,y) {
    // tạo một đối tượng (1)
    var instance = {};

    // khởi tạo thể hiện (3)
    instance.x = x;
    instance.y = y;

    // trả về thể hiện (4)
    return instance;
}

var point = Point2d(3,4);

point.x;                    // 3
point.y;                    // 4
```

Không có `class`, chỉ là một định nghĩa hàm thông thường (`Point2d(..)`). Không có lệnh gọi `new`, chỉ là một lệnh gọi hàm thông thường (`Point2d(3,4)`). Và không có tham chiếu `this`, chỉ là các gán thuộc tính đối tượng thông thường (`instance.x = ..`).

Thuật ngữ thường được sử dụng nhất để chỉ mẫu mã này là `Point2d(..)` ở đây là một *hàm factory* (factory function). Việc gọi nó gây ra việc xây dựng (tạo và khởi tạo) một đối tượng, và trả lại đối tượng đó cho chúng ta. Đó là một mẫu cực kỳ phổ biến, ít nhất cũng phổ biến như mã định hướng lớp.

Tôi đã chú thích nhận xét `(1)`, `(3)`, và `(4)` trong đoạn mã đó, tương ứng đại khái với các bước 1, 3 và 4 của hoạt động `new`. Nhưng bước 2 ở đâu?

Nếu bạn nhớ lại, bước 2 của `new` là về việc liên kết đối tượng (được tạo ở bước 1) với một đối tượng khác, thông qua khe `[[Prototype]]` của nó (xem Chương 2). Vậy chúng ta có thể muốn liên kết đối tượng `instance` của mình với đối tượng nào? Chúng ta có thể liên kết nó với một đối tượng chứa các hàm mà chúng ta muốn liên kết/sử dụng với thể hiện của mình.

Hãy sửa đổi đoạn mã trước:

```js
var prototypeObj = {
    toString() {
        return `(${this.x},${this.y})`;
    },
}

// một "constructor" không phải lớp
function Point2d(x,y) {
    // tạo một đối tượng (1)
    var instance = {
        // liên kết [[Prototype]] của thể hiện (2)
        __proto__: prototypeObj,
    };

    // khởi tạo thể hiện (3)
    instance.x = x;
    instance.y = y;

    // trả về thể hiện (4)
    return instance;
}

var point = Point2d(3,4);

point.toString();           // (3,4)
```

Bây giờ bạn thấy gán `__proto__` đang thiết lập liên kết `[[Prototype]]` nội bộ, đó là bước 2 còn thiếu. Tôi đã sử dụng `__proto__` ở đây chỉ nhằm mục đích minh họa; sử dụng `setPrototypeOf(..)` như được hiển thị trong Chương 4 sẽ hoàn thành nhiệm vụ tương tự.

### Thể Hiện Factory *Mới* (*New* Factory Instance)

Bạn nghĩ điều gì sẽ xảy ra nếu chúng ta sử dụng `new` để gọi hàm `Point2d(..)` như được hiển thị ở đây?

```js
var anotherPoint = new Point2d(5,6);

anotherPoint.toString(5,6);         // (5,6)
```

Chờ đã! Chuyện gì đang xảy ra ở đây? Một hàm factory thông thường, không phải `class` được gọi với từ khóa `new`, như thể nó là một `class`. Điều đó có thay đổi bất cứ điều gì về kết quả của mã không?

Không... và có. `anotherPoint` ở đây chính xác là cùng một đối tượng như thể tôi không sử dụng `new`. Nhưng! Đối tượng mà `new` tạo ra, liên kết và gán làm ngữ cảnh `this`? Đối tượng *đó* hoàn toàn bị bỏ qua và vứt đi, cuối cùng sẽ được bộ thu gom rác của JS thu hồi. Thật không may, công cụ JS không thể dự đoán rằng bạn sẽ không sử dụng đối tượng mà bạn đã yêu cầu `new` tạo ra, vì vậy nó luôn vẫn được tạo ra ngay cả khi nó không được sử dụng.

Đúng vậy! Sử dụng từ khóa `new` đối với một hàm factory có thể *cảm thấy* tiện dụng hoặc quen thuộc hơn, nhưng nó khá lãng phí, ở chỗ nó tạo ra **hai** đối tượng, và lãng phí vứt bỏ một trong số chúng.

### Khởi Tạo Factory (Factory Initialization)

Trong ví dụ mã hiện tại, hàm `Point2d(..)` vẫn trông rất giống một `constructor(..)` bình thường của một định nghĩa `class`. Nhưng điều gì sẽ xảy ra nếu chúng ta chuyển mã khởi tạo sang một hàm riêng biệt, giả sử có tên là `init(..)`:

```js
var prototypeObj = {
    init(x,y) {
        // khởi tạo thể hiện (3)
        this.x = x;
        this.y = y;
    },
    toString() {
        return `(${this.x},${this.y})`;
    },
}

// một "constructor" không phải lớp
function Point2d(x,y) {
    // tạo một đối tượng (1)
    var instance = {
        // liên kết [[Prototype]] của thể hiện (2)
        __proto__: prototypeObj,
    };

    // khởi tạo thể hiện (3)
    instance.init(x,y);

    // trả về thể hiện (4)
    return instance;
}

var point = Point2d(3,4);

point.toString();           // (3,4)
```

Lệnh gọi `instance.init(..)` sử dụng liên kết `[[Prototype]]` được thiết lập thông qua gán `__proto__`. Do đó, nó *ủy quyền* lên chuỗi nguyên mẫu đến `prototypeObj.init(..)`, và gọi nó với ngữ cảnh `this` của `instance` -- thông qua gán *ngữ cảnh ngầm định* (xem Chương 4).

Hãy tiếp tục giải cấu trúc. Hãy sẵn sàng cho một sự thay đổi!

```js
var Point2d = {
    init(x,y) {
        // khởi tạo thể hiện (3)
        this.x = x;
        this.y = y;
    },
    toString() {
        return `(${this.x},${this.y})`;
    },
};
```

Whoa, cái gì!? Tôi đã loại bỏ hàm `Point2d(..)`, và thay vào đó đổi tên `prototypeObj` thành `Point2d`. Kỳ lạ.

Nhưng hãy nhìn vào phần còn lại của mã bây giờ:

```js
// các bước 1, 2, và 4
var point = { __proto__: Point2d, };

// bước 3
point.init(3,4);

point.toString();           // (3,4)
```

Và một tinh chỉnh cuối cùng: hãy sử dụng một tiện ích tích hợp mà JS cung cấp cho chúng ta, gọi là `Object.create(..)`:

```js
// các bước 1, 2, và 4
var point = Object.create(Point2d);

// bước 3
point.init(3,4);

point.toString();           // (3,4)
```

`Object.create(..)` thực hiện những thao tác nào?

1. tạo một đối tượng rỗng hoàn toàn mới, từ hư không.

2. liên kết `[[Prototype]]` của đối tượng rỗng mới đó với đối tượng `.prototype` của hàm.

Nếu những điều đó trông quen thuộc, đó là bởi vì đó chính xác là hai bước đầu tiên giống nhau của từ khóa `new` (xem Chương 4).

Hãy đặt cái này lại với nhau ngay bây giờ:

```js
var Point2d = {
    init(x,y) {
        this.x = x;
        this.y = y;
    },
    toString() {
        return `(${this.x},${this.y})`;
    },
};

var point = Object.create(Point2d);

point.init(3,4);

point.toString();           // (3,4)
```

Hmmm. Hãy dành một chút thời gian để suy ngẫm về những gì đã được bắt nguồn ở đây. Nó so sánh như thế nào với cách tiếp cận `class`?

Mẫu này loại bỏ các từ khóa `class` và `new`, nhưng hoàn thành kết quả chính xác tương tự. *Chi phí*? Hoạt động `new` đơn lẻ đã được chia thành hai câu lệnh: `Object.create(Point2d)` và `point.init(3,4)`.

#### Giúp Tôi Tái Cấu Trúc! (Help Me Reconstruct!)

Nếu việc tách rời hai hoạt động đó làm phiền bạn -- liệu nó có *quá bị giải cấu trúc* không!? -- chúng luôn có thể được kết hợp lại trong một trình trợ giúp factory nhỏ:

```js
function make(objType,...args) {
    var instance = Object.create(objType);
    instance.init(...args);
    return instance;
}

var point = make(Point2d,3,4);

point.toString();           // (3,4)
```

| MẸO: |
| :--- |
| Một trình trợ giúp hàm factory `make(..)` như vậy hoạt động chung cho bất kỳ loại đối tượng nào, miễn là bạn tuân theo quy ước ngụ ý rằng mỗi `objType` bạn liên kết đến đều có một hàm tên là `init(..)` trên đó. |

Và tất nhiên, bạn vẫn có thể tạo bao nhiêu thể hiện tùy thích:

```js
var point = make(Point2d,3,4);

var anotherPoint = make(Point2d,5,6);
```

## Từ Bỏ Tư Duy Lớp (Ditching Class Thinking)

Thành thật mà nói, việc *giải cấu trúc* mà chúng ta vừa trải qua chỉ kết thúc bằng mã hơi khác một chút, và có thể tốt hơn một chút hoặc tệ hơn một chút, so với kiểu `class`. Nếu đó là tất cả những gì ủy quyền hướng tới, nó có lẽ thậm chí sẽ không đủ hữu ích cho nhiều hơn một chú thích, chứ đừng nói đến cả một chương.

Nhưng đây là nơi chúng ta sẽ thực sự bắt đầu đẩy tư duy định hướng lớp, không chỉ cú pháp, sang một bên.

Thiết kế định hướng lớp vốn dĩ tạo ra một hệ thống phân cấp *phân loại*, nghĩa là cách chúng ta phân chia và nhóm các đặc điểm, và sau đó xếp chồng chúng theo chiều dọc trong một chuỗi thừa kế. Hơn nữa, việc định nghĩa một lớp con là một sự chuyên biệt hóa của lớp cơ sở tổng quát. Khởi tạo là một sự chuyên biệt hóa của lớp tổng quát.

Hành vi trong một hệ thống phân cấp lớp truyền thống là một thành phần dọc thông qua các lớp của chuỗi thừa kế. Những nỗ lực đã được thực hiện trong nhiều thập kỷ, và thậm chí trở nên khá phổ biến vào những thời điểm nhất định, để làm phẳng các hệ thống phân cấp thừa kế sâu, và ủng hộ một thành phần ngang hơn thông qua *mixins* và các ý tưởng liên quan.

Tôi không khẳng định có bất cứ điều gì sai với những cách tiếp cận mã đó. Nhưng tôi đang nói rằng chúng không phải là cách JS hoạt động *tự nhiên*, vì vậy việc áp dụng chúng trong JS là một con đường dài, quanh co, phức tạp, và đã tích lũy nhiều cú pháp sắc thái khác nhau để trang bị thêm trên đỉnh trụ cột `[[Prototype]]` và `this` cốt lõi của JS.

Đối với phần còn lại của chương này, tôi dự định loại bỏ cả cú pháp của `class` *và* tư duy của *lớp*.

## Minh Họa Ủy Quyền (Delegation Illustrated)

Vậy ủy quyền là gì? Về cốt lõi, đó là về hai hoặc nhiều *thứ* chia sẻ nỗ lực hoàn thành một nhiệm vụ.

Thay vì định nghĩa một *thứ* cha chung `Point2d` đại diện cho hành vi được chia sẻ mà một tập hợp một hoặc nhiều *thứ* con `point` / `anotherPoint` kế thừa từ đó, ủy quyền chuyển chúng ta sang việc xây dựng chương trình của mình với các *thứ* ngang hàng rời rạc hợp tác với nhau.

Tôi sẽ phác thảo điều đó trong một số mã:

```js
var Coordinates = {
    setX(x) {
        this.x = x;
    },
    setY(y) {
        this.y = y;
    },
    setXY(x,y) {
        this.setX(x);
        this.setY(y);
    },
};

var Inspect = {
    toString() {
        return `(${this.x},${this.y})`;
    },
};

var point = {};

Coordinates.setXY.call(point,3,4);
Inspect.toString.call(point);         // (3,4)

var anotherPoint = Object.create(Coordinates);

anotherPoint.setXY(5,6);
Inspect.toString.call(anotherPoint);  // (5,6)
```

Hãy phân tích những gì đang xảy ra ở đây.

Tôi đã định nghĩa `Coordinates` là một đối tượng cụ thể chứa một số hành vi tôi liên kết với việc thiết lập tọa độ điểm (`x` và `y`). Tôi cũng đã định nghĩa `Inspect` là một đối tượng cụ thể chứa một số logic kiểm tra gỡ lỗi, chẳng hạn như `toString()`.

Sau đó, tôi tạo thêm hai đối tượng cụ thể nữa, `point` và `anotherPoint`.

`point` không có `[[Prototype]]` cụ thể (mặc định: `Object.prototype`). Sử dụng gán *ngữ cảnh rõ ràng* (xem Chương 4), tôi gọi các tiện ích `Coordinates.setXY(..)` và `Inspect.toString()` trong ngữ cảnh của `point`. Đó là những gì tôi gọi là *ủy quyền rõ ràng*.

`anotherPoint` được liên kết `[[Prototype]]` với `Coordinates`, chủ yếu để thuận tiện một chút. Điều đó cho phép tôi sử dụng gán *ngữ cảnh ngầm định* với `anotherPoint.setXY(..)`. Nhưng tôi vẫn có thể chia sẻ *rõ ràng* `anotherPoint` làm ngữ cảnh cho lệnh gọi `Inspect.toString()`. Đó là những gì tôi gọi là *ủy quyền ngầm định*.

**Đừng bỏ lỡ điều *này*:** Chúng ta vẫn hoàn thành việc soạn thảo (composition): chúng ta đã soạn thảo các hành vi từ `Coordinates` và `Inspect`, trong quá trình gọi hàm thời gian chạy với chia sẻ ngữ cảnh `this`. Chúng ta không cần phải kết hợp tác giả các hành vi đó vào một `class` duy nhất (hoặc hệ thống phân cấp `class` cơ sở-lớp con) để `point` / `anotherPoint` kế thừa. Tôi thích gọi thành phần thời gian chạy này là, **thành phần ảo** (virtual composition).

*Điểm* ở đây là: không có đối tượng nào trong số bốn đối tượng này là cha hoặc con. Tất cả chúng đều là ngang hàng của nhau, và tất cả đều có mục đích khác nhau. Chúng ta có thể tổ chức hành vi của mình thành các phần logic (trên mỗi đối tượng tương ứng), và chia sẻ ngữ cảnh thông qua `this` (và, tùy chọn liên kết `[[Prototype]]`), kết thúc với cùng kết quả thành phần như các mẫu khác mà chúng ta đã kiểm tra cho đến nay trong cuốn sách.

*Đó* là trái tim của mẫu **ủy quyền**, như JS thể hiện nó.

| MẸO: |
| :--- |
| Trong ấn bản đầu tiên của bộ sách này, cuốn sách này ("this & Object Prototypes") đã đặt ra một thuật ngữ, "OLOO", viết tắt của "Objects Linked to Other Objects" (Các đối tượng được liên kết với các đối tượng khác) -- để đứng đối lập với "OO" ("Object Oriented" - Hướng đối tượng). Trong ví dụ trước, bạn có thể thấy bản chất của OLOO: tất cả những gì chúng ta có là các đối tượng, được liên kết và hợp tác với, các đối tượng khác. Tôi thấy điều này đẹp trong sự đơn giản của nó. |

## Soạn Thảo Các Đối Tượng Ngang Hàng (Composing Peer Objects)

Hãy đưa *ủy quyền này* đi xa hơn nữa.

Trong đoạn mã trước, `point` và `anotherPoint` chỉ đơn thuần giữ dữ liệu, và các hành vi mà chúng ủy quyền nằm trên các đối tượng khác (`Coordinates` và `Inspect`). Nhưng chúng ta có thể thêm các hành vi trực tiếp vào bất kỳ đối tượng nào trong chuỗi ủy quyền, và các hành vi đó thậm chí có thể tương tác với nhau, tất cả thông qua sự kỳ diệu của *thành phần ảo* (chia sẻ ngữ cảnh `this`).

Để minh họa, chúng ta sẽ phát triển ví dụ *điểm* hiện tại của mình lên một chút. Và như một phần thưởng, chúng ta thực sự sẽ vẽ các điểm của mình trên một phần tử `<canvas>` trong DOM. Hãy xem:

```js
var Canvas = {
    setOrigin(x,y) {
        this.ctx.translate(x,y);

        // lật ngữ cảnh canvas theo chiều dọc,
        // để tọa độ hoạt động giống như trên
        // biểu đồ 2d (x,y) bình thường
        this.ctx.scale(1,-1);
    },
    pixel(x,y) {
        this.ctx.fillRect(x,y,1,1);
    },
    renderScene() {
        // xóa canvas
        var matrix = this.ctx.getTransform();
        this.ctx.resetTransform();
        this.ctx.clearRect(
            0, 0,
            this.ctx.canvas.width,
            this.ctx.canvas.height
        );
        this.ctx.setTransform(matrix);

        this.draw();  // <-- draw() ở đâu?
    },
};

var Coordinates = {
    setX(x) {
        this.x = Math.round(x);
    },
    setY(y) {
        this.y = Math.round(y);
    },
    setXY(x,y) {
        this.setX(x);
        this.setY(y);
        this.render();   // <-- render() ở đâu?
    },
};

var ControlPoint = {
    // ủy quyền cho Coordinates
    __proto__: Coordinates,

    // LƯU Ý: phải có một phần tử <canvas id="my-canvas">
    // trong DOM
    ctx: document.getElementById("my-canvas")
        .getContext("2d"),

    rotate(angleRadians) {
        var rotatedX = this.x * Math.cos(angleRadians) -
            this.y * Math.sin(angleRadians);
        var rotatedY = this.x * Math.sin(angleRadians) +
            this.y * Math.cos(angleRadians);
        this.setXY(rotatedX,rotatedY);
    },
    draw() {
        // vẽ điểm
        Canvas.pixel.call(this,this.x,this.y);
    },
    render() {
        // xóa canvas, và hiển thị lại
        // control-point của chúng ta
        Canvas.renderScene.call(this);
    },
};

// đặt gốc tọa độ logic (0,0) tại vị trí
// vật lý này trên canvas
Canvas.setOrigin.call(ControlPoint,100,100);

ControlPoint.setXY(30,40);
// [hiển thị điểm (30,40) trên canvas]

// ..
// sau đó:

// xoay điểm quanh gốc tọa độ (0,0)
// 90 độ ngược chiều kim đồng hồ
ControlPoint.rotate(Math.PI / 2);
// [hiển thị điểm (-40,30) trên canvas]
```

OK, đó là rất nhiều mã để tiêu hóa. Hãy dành thời gian của bạn và đọc lại đoạn mã nhiều lần. Tôi đã thêm một vài đối tượng cụ thể mới (`Canvas` và `ControlPoint`) cùng với đối tượng `Coordinates` trước đó.

Hãy chắc chắn rằng bạn nhìn thấy và hiểu sự tương tác giữa ba đối tượng cụ thể này.

`ControlPoint` được liên kết (thông qua `__proto__`) để *ủy quyền ngầm định* (chuỗi `[[Prototype]]`) cho `Coordinates`.

Đây là một *ủy quyền rõ ràng*: `Canvas.setOrigin.call(ControlPoint,100,100);`; Tôi đang gọi lệnh gọi `Canvas.setOrigin(..)` trong ngữ cảnh của `ControlPoint`. Điều đó có tác dụng chia sẻ `ctx` với `setOrigin(..)`, thông qua `this`.

`ControlPoint.setXY(..)` ủy quyền *ngầm định* cho `Coordinates.setXY(..)`, nhưng vẫn trong ngữ cảnh của `ControlPoint`. Đây là một chi tiết quan trọng dễ bị bỏ qua: bạn có thấy `this.render()` bên trong `Coordinates.setXY(..)` không? Nó đến từ đâu? Vì ngữ cảnh `this` là `ControlPoint` (không phải `Coordinates`), nó đang gọi `ControlPoint.render()`.

`ControlPoint.render()` *ủy quyền rõ ràng* cho `Canvas.renderScene()`, một lần nữa vẫn trong ngữ cảnh `ControlPoint`. `renderScene()` gọi `this.draw()`, nhưng nó đến từ đâu? Đúng vậy, vẫn từ `ControlPoint` (thông qua ngữ cảnh `this`).

Và `ControlPoint.draw()`? Nó *ủy quyền rõ ràng* cho `Canvas.pixel(..)`, lại một lần nữa vẫn trong ngữ cảnh `ControlPoint`.

Cả ba đối tượng đều có các phương thức kết thúc bằng việc gọi lẫn nhau. Nhưng các cuộc gọi này không đặc biệt được nối cứng. `Canvas.renderScene()` không gọi `ControlPoint.draw()`, nó gọi `this.draw()`. Điều đó quan trọng, bởi vì nó có nghĩa là `Canvas.renderScene()` linh hoạt hơn để sử dụng trong một ngữ cảnh `this` khác -- ví dụ, đối với một loại đối tượng *điểm* khác ngoài `ControlPoint`.

Chính thông qua ngữ cảnh `this`, và chuỗi `[[Prototype]]`, ba đối tượng này về cơ bản được trộn lẫn (soạn thảo) ảo cùng nhau, khi cần thiết ở mỗi bước, để chúng hoạt động cùng nhau **như thể chúng là một đối tượng thay vì ba đối tượng riêng biệt**.

Đó là *vẻ đẹp* của thành phần ảo được hiện thực hóa bởi mẫu ủy quyền trong JS.

### Ngữ Cảnh Linh Hoạt (Flexible Context)

Tôi đã đề cập ở trên rằng chúng ta có thể khá dễ dàng thêm các đối tượng cụ thể khác vào hỗn hợp. Đây là một ví dụ:

```js
var Coordinates = { /* .. */ };

var Canvas = {
    /* .. */
    line(start,end) {
        this.ctx.beginPath();
        this.ctx.moveTo(start.x,start.y);
        this.ctx.lineTo(end.x,end.y);
        this.ctx.stroke();
    },
};

function lineAnchor(x,y) {
    var anchor = {
        __proto__: Coordinates,
        render() {},
    };
    anchor.setXY(x,y);
    return anchor;
}

var GuideLine = {
    // LƯU Ý: phải có một phần tử <canvas id="my-canvas">
    // trong DOM
    ctx: document.getElementById("my-canvas")
        .getContext("2d"),

    setAnchors(sx,sy,ex,ey) {
        this.start = lineAnchor(sx,sy);
        this.end = lineAnchor(ex,ey);
        this.render();
    },
    draw() {
        // vẽ điểm
        Canvas.line.call(this,this.start,this.end);
    },
    render() {
        // xóa canvas, và hiển thị lại
        // dòng của chúng ta
        Canvas.renderScene.call(this);
    },
};

// đặt gốc tọa độ logic (0,0) tại vị trí
// vật lý này trên canvas
Canvas.setOrigin.call(GuideLine,100,100);

GuideLine.setAnchors(-30,65,45,-17);
// [hiển thị dòng từ (-30,65) đến (45,-17)
//   trên canvas]
```

Tôi nghĩ điều đó khá tuyệt!

Nhưng tôi nghĩ một lợi ích khác ít rõ ràng hơn là việc có các đối tượng được liên kết động thông qua ngữ cảnh `this` có xu hướng làm cho việc kiểm tra các phần khác nhau của chương trình một cách độc lập, phần nào dễ dàng hơn.

Ví dụ, `Object.setPrototypeOf(..)` có thể được sử dụng để thay đổi động liên kết `[[Prototype]]` của một đối tượng, ủy quyền nó cho một đối tượng khác như một đối tượng giả (mock object). Hoặc bạn có thể định nghĩa lại động `GuideLine.draw()` và `GuideLine.render()` để *ủy quyền rõ ràng* cho một `MockCanvas` thay vì `Canvas`.

Từ khóa `this`, và liên kết `[[Prototype]]`, là một cơ chế cực kỳ linh hoạt khi bạn hiểu và tận dụng chúng một cách đầy đủ.

## Tại Sao Là *This*? (Why *This*?)

OK, vì vậy hy vọng rõ ràng là mẫu ủy quyền dựa nhiều vào đầu vào ngầm định, chia sẻ ngữ cảnh thông qua `this` thay vì thông qua một tham số rõ ràng.

Bạn có thể hỏi một cách đúng đắn, tại sao không chỉ luôn truyền ngữ cảnh đó xung quanh một cách rõ ràng? Chúng ta chắc chắn có thể làm như vậy, nhưng... để truyền ngữ cảnh cần thiết theo cách thủ công, chúng ta sẽ phải thay đổi khá nhiều chữ ký hàm đơn lẻ, và bất kỳ vị trí gọi tương ứng nào.

Hãy xem lại ví dụ ủy quyền `ControlPoint` trước đó, và triển khai nó mà không có bất kỳ chia sẻ ngữ cảnh `this` định hướng ủy quyền nào. Hãy chú ý cẩn thận đến sự khác biệt:

```js
var Canvas = {
    setOrigin(ctx,x,y) {
        ctx.translate(x,y);
        ctx.scale(1,-1);
    },
    pixel(ctx,x,y) {
        ctx.fillRect(x,y,1,1);
    },
    renderScene(ctx,entity) {
        // xóa canvas
        var matrix = ctx.getTransform();
        ctx.resetTransform();
        ctx.clearRect(
            0, 0,
            ctx.canvas.width,
            ctx.canvas.height
        );
        ctx.setTransform(matrix);

        entity.draw();
    },
};

var Coordinates = {
    setX(entity,x) {
        entity.x = Math.round(x);
    },
    setY(entity,y) {
        entity.y = Math.round(y);
    },
    setXY(entity,x,y) {
        this.setX(entity,x);
        this.setY(entity,y);
        entity.render();
    },
};

var ControlPoint = {
    // LƯU Ý: phải có một phần tử <canvas id="my-canvas">
    // trong DOM
    ctx: document.getElementById("my-canvas")
        .getContext("2d"),

    setXY(x,y) {
        Coordinates.setXY(this,x,y);
    },
    rotate(angleRadians) {
        var rotatedX = this.x * Math.cos(angleRadians) -
            this.y * Math.sin(angleRadians);
        var rotatedY = this.x * Math.sin(angleRadians) +
            this.y * Math.cos(angleRadians);
        this.setXY(rotatedX,rotatedY);
    },
    draw() {
        // vẽ điểm
        Canvas.pixel(this.ctx,this.x,this.y);
    },
    render() {
        // xóa canvas, và hiển thị lại
        // control-point của chúng ta
        Canvas.renderScene(this.ctx,this);
    },
};

// đặt gốc tọa độ logic (0,0) tại vị trí
// vật lý này trên canvas
Canvas.setOrigin(ControlPoint.ctx,100,100);

// ..
```

Thành thật mà nói, một số bạn có thể thích phong cách mã đó hơn. Và điều đó ổn nếu bạn ở trong phe đó. Đoạn mã này tránh `[[Prototype]]` hoàn toàn, và chỉ dựa vào ít hơn nhiều các tham chiếu kiểu `this.` cơ bản đến các thuộc tính và phương thức.

Ngược lại, phong cách ủy quyền mà tôi đang ủng hộ trong chương này là không quen thuộc và sử dụng chia sẻ `[[Prototype]]` và `this` theo những cách mà bạn có thể không quen thuộc. Để sử dụng một phong cách như vậy một cách hiệu quả, bạn sẽ phải đầu tư thời gian và thực hành để xây dựng sự quen thuộc sâu sắc hơn.

Nhưng theo ý kiến của tôi, "chi phí" của việc tránh thành phần ảo thông qua ủy quyền có thể được cảm nhận trên tất cả các chữ ký hàm và vị trí gọi; tôi thấy chúng lộn xộn hơn nhiều. Việc truyền ngữ cảnh rõ ràng đó là một khoản thuế khá lớn.

Trên thực tế, tôi sẽ không bao giờ ủng hộ phong cách mã đó. Nếu bạn muốn tránh ủy quyền, có lẽ tốt nhất là chỉ nên gắn bó với mã kiểu `class`, như đã thấy trong Chương 3. Như một bài tập dành cho người đọc, hãy thử chuyển đổi các đoạn mã `ControlPoint` / `GuideLine` trước đó để sử dụng `class`.

[^TreatyOfOrlando]: "Treaty of Orlando"; Henry Lieberman, Lynn Andrea Stein, David Ungar; Oct 6, 1987; https://web.media.mit.edu/~lieber/Publications/Treaty-of-Orlando-Treaty-Text.pdf ; PDF; Accessed July 2022

[^ClassVsPrototype]: "Classes vs. Prototypes, Some Philosophical and Historical Observations"; Antero Taivalsaari; Apr 22, 1996; https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.56.4713&rep=rep1&type=pdf ; PDF; Accessed July 2022
