# You Don't Know JS Yet: Đối tượng & Lớp - Ấn bản thứ 2
# Chương 3: Đối tượng Classy

| LƯU Ý: |
| :--- |
| Đang trong quá trình thực hiện |

Mẫu thiết kế lớp (class-design pattern) thường đòi hỏi việc định nghĩa một *loại sự vật* (lớp), bao gồm dữ liệu (thành viên) và hành vi (phương thức), và sau đó tạo một hoặc nhiều *thể hiện* cụ thể của định nghĩa lớp này dưới dạng các đối tượng thực tế có thể tương tác và thực hiện các tác vụ. Hơn nữa, định hướng lớp cho phép khai báo mối quan hệ giữa hai hoặc nhiều lớp, thông qua cái gọi là "kế thừa", để tạo ra các "lớp con" mới và được tăng cường, kết hợp và thậm chí định nghĩa lại các hành vi.

Trước ES6 (2015), các nhà phát triển JS đã bắt chước các khía cạnh của thiết kế hướng lớp (hay còn gọi là "hướng đối tượng") bằng cách sử dụng các hàm và đối tượng đơn giản, cùng với cơ chế `[[Prototype]]` (như đã giải thích trong chương trước) -- cái gọi là "lớp nguyên mẫu" (prototypal classes).

Nhưng trước sự vui mừng và nhẹ nhõm của nhiều nhà phát triển, ES6 đã giới thiệu cú pháp chuyên dụng, bao gồm các từ khóa `class` và `extends`, để thể hiện thiết kế hướng lớp một cách khai báo hơn.

Vào thời điểm `class` của ES6 được giới thiệu, cú pháp chuyên dụng mới này gần như hoàn toàn *chỉ là đường cú pháp* (syntactic sugar) để làm cho các định nghĩa lớp thuận tiện và dễ đọc hơn. Tuy nhiên, trong nhiều năm kể từ ES6, `class` đã trưởng thành và phát triển thành cơ chế tính năng hạng nhất của riêng nó, tích lũy một lượng đáng kể cú pháp chuyên dụng và các hành vi phức tạp vượt xa khả năng "lớp nguyên mẫu" trước ES6.

Mặc dù `class` hiện nay gần như không có sự tương đồng nào với phong cách mã "lớp nguyên mẫu" cũ hơn, công cụ JS vẫn *chỉ* đang nối các đối tượng với nhau thông qua cơ chế `[[Prototype]]` hiện có. Nói cách khác, `class` không phải là trụ cột riêng biệt của ngôn ngữ (như `[[Prototype]]`), mà giống như *Đầu cột* trang trí, lạ mắt nằm trên đỉnh trụ cột/cột.

Điều đó nói rằng, vì mã kiểu `class` hiện đã thay thế hầu như tất cả mã hóa "lớp nguyên mẫu" trước đây, văn bản chính ở đây chỉ tập trung vào `class` và các chi tiết khác nhau của nó. Vì mục đích lịch sử, chúng tôi sẽ đề cập ngắn gọn về phong cách "lớp nguyên mẫu" cũ trong một phụ lục.

## Khi Nào Tôi Nên Định Hướng Lớp Cho Mã Của Mình?

Định hướng lớp là một mẫu thiết kế, có nghĩa là đó là một lựa chọn cho cách bạn tổ chức thông tin và hành vi trong chương trình của mình. Nó có ưu và nhược điểm. Nó không phải là giải pháp phổ quát cho mọi tác vụ.

Vậy làm thế nào để bạn biết khi nào bạn nên sử dụng các lớp?

Theo nghĩa lý thuyết, định hướng lớp là một cách chia miền nghiệp vụ của một chương trình thành một hoặc nhiều phần, mỗi phần có thể được định nghĩa bởi một phân loại "là một" (is-a): nhóm một thứ vào tập hợp (hoặc các tập hợp) các đặc điểm mà thứ đó chia sẻ với những thứ tương tự khác. Bạn sẽ nói "X là một Y", nghĩa là X có (ít nhất) tất cả các đặc điểm của một thứ thuộc loại Y.

Ví dụ, hãy xem xét máy tính. Chúng ta có thể nói một máy tính là thiết bị điện, vì nó sử dụng dòng điện (điện áp, ampe, v.v.) làm năng lượng. Hơn nữa, nó là thiết bị điện tử, bởi vì nó thao tác dòng điện vượt ra ngoài việc chỉ đơn giản là định tuyến các electron xung quanh (trường điện/từ), tạo ra một mạch có ý nghĩa để thao tác dòng điện thành việc thực hiện các tác vụ phức tạp hơn. Ngược lại, một chiếc đèn bàn cơ bản là thiết bị điện, nhưng không thực sự là thiết bị điện tử.

Do đó, chúng ta có thể định nghĩa một lớp `Electrical` để mô tả những gì các thiết bị điện cần và có thể làm. Sau đó, chúng ta có thể định nghĩa thêm một lớp `Electronic`, và định nghĩa rằng ngoài việc là thiết bị điện, những thứ `Electronic` thao tác điện để tạo ra các kết quả chuyên biệt hơn.

Đây là nơi định hướng lớp bắt đầu tỏa sáng. Thay vì định nghĩa lại tất cả các đặc điểm `Electrical` trong lớp `Electronic`, chúng ta có thể định nghĩa `Electronic` theo cách mà nó "chia sẻ" hoặc "kế thừa" các đặc điểm đó từ `Electrical`, và sau đó tăng cường/định nghĩa lại các hành vi độc đáo làm cho một thiết bị trở thành điện tử. Mối quan hệ này giữa hai lớp -- được gọi là "kế thừa" -- là một khía cạnh chính của định hướng lớp.

Vì vậy, định hướng lớp là một cách suy nghĩ về các thực thể mà chương trình của chúng ta cần, và phân loại chúng thành các nhóm dựa trên các đặc điểm của chúng (chúng giữ thông tin gì, những thao tác nào có thể được thực hiện trên dữ liệu đó), và định nghĩa các mối quan hệ giữa các nhóm đặc điểm khác nhau.

Nhưng chuyển từ lý thuyết sang một góc nhìn thực tế hơn một chút: nếu chương trình của bạn cần giữ và sử dụng nhiều bộ sưu tập (thể hiện) của dữ liệu/hành vi giống nhau cùng một lúc, bạn *có thể* hưởng lợi từ định hướng lớp.

### Thời Gian Cho Một Ví Dụ

Đây là một minh họa ngắn.

Một vài thập kỷ trước, ngay sau khi tôi đã trải qua gần như tất cả bằng Cử nhân Khoa học Máy tính ở trường đại học, tôi thấy mình đang ngồi trong công việc phát triển phần mềm chuyên nghiệp đầu tiên của mình. Tôi được giao nhiệm vụ xây dựng, hoàn toàn một mình, một hệ thống theo dõi bảng chấm công và tính lương. Tôi đã xây dựng phần backend bằng PHP (sử dụng MySQL cho DB) và sử dụng JS cho giao diện (khi nó còn sơ khai vào khoảng đầu thế kỷ).

Vì bằng CS của tôi đã nhấn mạnh định hướng lớp rất nhiều trong suốt các khóa học của tôi, tôi rất háo hức đưa tất cả lý thuyết đó vào công việc. Đối với thiết kế chương trình của mình, tôi đã định nghĩa khái niệm về một thực thể "bảng chấm công" là một tập hợp của 2-3 thực thể "tuần", và mỗi "tuần" là một tập hợp của 5-7 thực thể "ngày", và mỗi "ngày" là một tập hợp các thực thể "tác vụ".

Nếu tôi muốn biết bao nhiêu giờ đã được ghi vào một thể hiện bảng chấm công, tôi có thể gọi một thao tác `totalTime()` trên thể hiện đó. Bảng chấm công định nghĩa thao tác này bằng cách lặp qua bộ sưu tập các tuần của nó, gọi `totalTime()` trên mỗi tuần và cộng tổng các giá trị. Mỗi tuần làm điều tương tự cho tất cả các ngày của nó, và mỗi ngày làm điều tương tự cho tất cả các tác vụ của nó.

Khái niệm được minh họa ở đây, một trong những nguyên tắc cơ bản của các mẫu thiết kế như định hướng lớp, được gọi là *đóng gói* (encapsulation). Mỗi cấp độ thực thể đóng gói (ví dụ: kiểm soát, ẩn, trừu tượng hóa) các chi tiết nội bộ (dữ liệu và hành vi) trong khi trình bày một giao diện bên ngoài hữu ích.

Nhưng đóng gói một mình không phải là một lý do đủ cho định hướng lớp. Các mẫu thiết kế khác cung cấp sự đóng gói đầy đủ.

Thiết kế lớp của tôi đã tận dụng sự kế thừa như thế nào? Tôi có một lớp cơ sở định nghĩa một tập hợp các thao tác như `totalTime()`, và mỗi loại lớp thực thể của tôi mở rộng/phân lớp lớp cơ sở này. Điều đó có nghĩa là mỗi lớp trong số chúng đều kế thừa khả năng tổng hợp tổng thời gian này, nhưng trong đó mỗi lớp áp dụng các phần mở rộng và định nghĩa riêng của chúng cho các chi tiết nội bộ về *cách* thực hiện công việc đó.

Còn một khía cạnh khác của mẫu thiết kế đang diễn ra, đó là *thành phần* (composition): mỗi thực thể được định nghĩa là một tập hợp của các thực thể khác.

### Đơn lẻ so với Nhiều

Tôi đã đề cập ở trên rằng một cách thực tế để quyết định xem bạn có cần định hướng lớp hay không là nếu chương trình của bạn sẽ có nhiều thể hiện của một loại/kiểu hành vi duy nhất (hay còn gọi là "lớp"). Trong ví dụ về bảng chấm công, chúng ta có 4 lớp: Timesheet, Week, Day, và Task. Nhưng đối với mỗi lớp, chúng ta có nhiều thể hiện của mỗi lớp cùng một lúc.

Nếu thay vào đó chúng ta chỉ cần một thể hiện duy nhất của một lớp, giống như chỉ một thứ `Computer` là một thể hiện của lớp `Electronic`, vốn là một lớp con của lớp `Electrical`, thì định hướng lớp có thể không mang lại nhiều lợi ích như vậy. Đặc biệt, nếu chương trình không cần tạo một thể hiện của lớp `Electrical`, thì không có lợi ích cụ thể nào khi tách `Electrical` khỏi `Electronic`, vì vậy chúng ta không thực sự nhận được bất kỳ sự trợ giúp nào từ khía cạnh kế thừa của định hướng lớp.

Vì vậy, nếu bạn thấy mình đang thiết kế một chương trình bằng cách chia miền vấn đề kinh doanh thành các "lớp" thực thể khác nhau, nhưng trong mã thực tế của chương trình, bạn chỉ cần một *thứ* cụ thể của một loại/định nghĩa hành vi (hay còn gọi là "lớp"), bạn rất có thể thực sự không cần định hướng lớp. Có các mẫu thiết kế khác có thể phù hợp hiệu quả hơn với nỗ lực của bạn.

Nhưng nếu bạn thấy mình muốn định nghĩa các lớp, và các lớp con kế thừa từ chúng, và nếu bạn sẽ khởi tạo một hoặc nhiều lớp đó nhiều lần, thì định hướng lớp là một ứng cử viên sáng giá. Và để thực hiện định hướng lớp trong JS, bạn sẽ cần từ khóa `class`.

## Giữ Cho Nó `class`y

`class` định nghĩa một khai báo hoặc biểu thức cho một lớp. Là một khai báo, một định nghĩa lớp xuất hiện ở vị trí câu lệnh và trông giống như thế này:

```js
class Point2d {
    // ..
}
```

Là một biểu thức, một định nghĩa lớp xuất hiện ở vị trí giá trị và có thể có tên hoặc ẩn danh:

```js
// biểu thức lớp có tên
const pointClass = class Point2d {
    // ..
};

// biểu thức lớp ẩn danh
const anotherClass = class {
    // ..
};
```

Nội dung của một thân `class` thường bao gồm một hoặc nhiều định nghĩa phương thức:

```js
class Point2d {
    setX(x) {
        // ..
    }
    setY(y) {
        // ..
    }
}
```

Bên trong một thân `class`, các phương thức được định nghĩa mà không có từ khóa `function`, và không có dấu phân cách `,` hoặc `;` giữa các định nghĩa phương thức.

| LƯU Ý: |
| :--- |
| Bên trong một khối `class`, tất cả mã chạy trong chế độ nghiêm ngặt (strict-mode) ngay cả khi không có chỉ thị `"use strict"` hiện diện trong tệp hoặc các hàm của nó. Đặc biệt, điều này ảnh hưởng đến hành vi của `this` đối với các cuộc gọi hàm, như được giải thích trong Chương 4. |

### Constructor

Một phương thức đặc biệt mà tất cả các lớp đều có được gọi là "constructor". Nếu bị bỏ qua, một constructor rỗng mặc định được giả định trong định nghĩa.

Constructor được gọi bất cứ khi nào một thể hiện `new` của lớp được tạo:

```js
class Point2d {
    constructor() {
        console.log("Đây là thể hiện mới của bạn!");
    }
}

var point = new Point2d();
// Đây là thể hiện mới của bạn!
```

Mặc dù cú pháp ngụ ý một hàm thực sự có tên `constructor` tồn tại, JS định nghĩa một hàm như được chỉ định, nhưng với tên của lớp (`Point2d` ở trên):

```js
typeof Point2d;       // "function"
```

Tuy nhiên, nó không *chỉ* là một hàm thông thường; loại hàm đặc biệt này hành xử hơi khác một chút:

```js
Point2d.toString();
// class Point2d {
//   ..
// }

Point2d();
// TypeError: Class constructor Point2d cannot
// be invoked without 'new'

Point2d.call({});
// TypeError: Class constructor Point2d cannot
// be invoked without 'new'
```

Bạn có thể xây dựng bao nhiêu thể hiện khác nhau của một lớp tùy ý:

```js
var one = new Point2d();
var two = new Point2d();
var three = new Point2d();
```

Mỗi `one`, `two`, và `three` ở đây là các đối tượng là các thể hiện độc lập của lớp `Point2d`.

| LƯU Ý: |
| :--- |
| Mỗi đối tượng `one`, `two`, và `three` có một liên kết `[[Prototype]]` đến đối tượng `Point2d.prototype` (xem Chương 2). Trong mã này, `Point2d` vừa là một định nghĩa `class` vừa là hàm constructor cùng tên. |

Nếu bạn thêm một thuộc tính vào đối tượng `one`:

```js
one.value = 42;
```

Thuộc tính đó bây giờ chỉ tồn tại trên `one`, và không tồn tại theo bất kỳ cách nào mà các đối tượng `two` hoặc `three` độc lập có thể truy cập:

```js
two.value;      // undefined
three.value;    // undefined
```

### Phương thức Lớp

Như đã trình bày ở trên, một định nghĩa lớp có thể bao gồm một hoặc nhiều định nghĩa phương thức:

```js
class Point2d {
    constructor() {
        console.log("Đây là thể hiện mới của bạn!");
    }
    setX(x) {
        console.log(`Đang đặt x thành: ${x}`);
        // ..
    }
}

var point = new Point2d();

point.setX(3);
// Đang đặt x thành: 3
```

Thuộc tính (phương thức) `setX` *trông giống như* nó tồn tại trên (được sở hữu bởi) đối tượng `point` ở đây. Nhưng đó là một ảo ảnh. Mỗi phương thức lớp được thêm vào đối tượng `prototype`, một thuộc tính của hàm constructor.

Vì vậy, `setX(..)` chỉ tồn tại dưới dạng `Point2d.prototype.setX`. Vì `point` được liên kết `[[Prototype]]` với `Point2d.prototype` (xem Chương 2) thông qua khởi tạo từ khóa `new`, tham chiếu `point.setX(..)` duyệt qua chuỗi `[[Prototype]]` và tìm phương thức để thực thi.

Các phương thức lớp chỉ nên được gọi thông qua một thể hiện; `Point2d.setX(..)` không hoạt động vì *không có* thuộc tính nào như vậy. Bạn *có thể* gọi `Point2d.prototype.setX(..)`, nhưng điều đó thường không đúng/được khuyên trong mã hóa hướng lớp tiêu chuẩn. Luôn truy cập các phương thức lớp thông qua các thể hiện.

## `this` Của Thể Hiện Lớp

Chúng ta sẽ đề cập đến từ khóa `this` chi tiết hơn nhiều trong một chương tiếp theo. Nhưng vì nó liên quan đến mã hướng lớp, từ khóa `this` thường đề cập đến thể hiện hiện tại là ngữ cảnh của bất kỳ lệnh gọi phương thức nào.

Trong constructor, cũng như bất kỳ phương thức nào, bạn có thể sử dụng `this.` để thêm hoặc truy cập các thuộc tính trên thể hiện hiện tại:

```js
class Point2d {
    constructor(x,y) {
        // thêm các thuộc tính vào thể hiện hiện tại
        this.x = x;
        this.y = y;
    }
    toString() {
        // truy cập các thuộc tính từ thể hiện hiện tại
        console.log(`(${this.x},${this.y})`);
    }
}

var point = new Point2d(3,4);

point.x;                // 3
point.y;                // 4

point.toString();       // (3,4)
```

Bất kỳ thuộc tính nào không giữ giá trị hàm, được thêm vào một thể hiện lớp (thường thông qua constructor), được gọi là *thành viên* (members), trái ngược với thuật ngữ *phương thức* (methods) cho các hàm có thể thực thi.

Trong khi phương thức `point.toString()` đang chạy, tham chiếu `this` của nó đang trỏ vào cùng một đối tượng mà `point` tham chiếu. Đó là lý do tại sao cả `point.x` và `this.x` đều tiết lộ cùng một giá trị `3` mà constructor đã đặt với thao tác `this.x = x` của nó.

### Trường Công khai (Public Fields)

Thay vì định nghĩa một thành viên thể hiện lớp một cách mệnh lệnh thông qua `this.` trong constructor hoặc một phương thức, các lớp có thể định nghĩa khai báo các *trường* trong thân `class`, tương ứng trực tiếp với các thành viên sẽ được tạo trên mỗi thể hiện:

```js
class Point2d {
    // đây là các trường công khai
    x = 0
    y = 0

    constructor(x,y) {
        // đặt các thuộc tính (trường) trên thể hiện hiện tại
        this.x = x;
        this.y = y;
    }
    toString() {
        // truy cập các thuộc tính từ thể hiện hiện tại
        console.log(`(${this.x},${this.y})`);
    }
}
```

Các trường công khai có thể có khởi tạo giá trị, như được hiển thị ở trên, nhưng điều đó không bắt buộc. Nếu bạn không khởi tạo một trường trong định nghĩa lớp, bạn hầu như luôn nên khởi tạo nó trong constructor.

Các trường cũng có thể tham chiếu lẫn nhau, thông qua cú pháp truy cập `this.` tự nhiên:

```js
class Point3d {
    // đây là các trường công khai
    x
    y = 4
    z = this.y * 5

    // ..
}
```

| MẸO: |
| :--- |
| Bạn chủ yếu có thể nghĩ về các khai báo trường công khai như thể chúng xuất hiện ở đầu `constructor(..)`, mỗi cái được bắt đầu bằng một `this.` ngụ ý mà bạn có thể bỏ qua trong dạng thân `class` khai báo. Nhưng, có một điểm đáng chú ý! Xem "Thật Siêu!" (That's Super!) sau để biết thêm thông tin về nó. |

Giống như tên thuộc tính được tính toán (xem Chương 1), tên trường có thể được tính toán:

```js
var coordName = "x";

class Point2d {
    // trường công khai được tính toán
    [coordName.toUpperCase()] = 42

    // ..
}

var point = new Point2d(3,4);

point.x;        // 3
point.y;        // 4

point.X;        // 42
```

#### Tránh Điều Này

Một mẫu đã xuất hiện và phát triển khá phổ biến, nhưng tôi tin chắc là một anti-pattern cho `class`, trông giống như sau:

```js
class Point2d {
    x = null
    y = null
    getDoubleX = () => this.x * 2

    constructor(x,y) {
        this.x = x;
        this.y = y;
    }
    toString() { /* .. */ }
}

var point = new Point2d(3,4);

point.getDoubleX();    // 6
```

Thấy trường giữ một hàm mũi tên `=>` không? Tôi nói đây là điều không nên. Nhưng tại sao? Hãy giải mã những gì đang diễn ra.

Đầu tiên, tại sao làm điều này? Bởi vì các nhà phát triển JS dường như luôn thất vọng bởi các quy tắc ràng buộc `this` động (xem Chương 4), vì vậy họ buộc một ràng buộc `this` thông qua hàm mũi tên `=>`. Bằng cách đó, bất kể `getDoubleX()` được gọi như thế nào, nó luôn được ràng buộc `this` với thể hiện cụ thể. Đó là một sự tiện lợi dễ hiểu để mong muốn, nhưng... nó phản bội bản chất của trụ cột `this` / `[[Prototype]]` của ngôn ngữ. Như thế nào?

Hãy xem xét mã tương đương với đoạn mã trước:

```js
class Point2d {
    constructor(x,y) {
        this.x = null;
        this.y = null;
        this.getDoubleX = () => this.x * 2;

        this.x = x;
        this.y = y;
    }
    toString() { /* .. */ }
}

var point = new Point2d(3,4);

point.getDoubleX();    // 6
```

Bạn có thể phát hiện ra vấn đề không? Nhìn kỹ. Tôi sẽ đợi.

...

Chúng tôi đã làm rõ nhiều lần cho đến nay rằng các định nghĩa `class` đặt các phương thức của chúng trên đối tượng `prototype` của hàm constructor lớp -- đó là nơi chúng thuộc về! -- sao cho chỉ có một hàm duy nhất và nó được kế thừa (chia sẻ) bởi tất cả các thể hiện. Đó là những gì sẽ xảy ra với `toString()` trong đoạn mã trên.

Nhưng còn `getDoubleX()` thì sao? Về cơ bản đó là một phương thức lớp, nhưng nó sẽ không được JS xử lý hoàn toàn giống như `toString()` sẽ làm. Hãy xem xét:

```js
Object.hasOwn(point,"x");               // true -- tốt
Object.hasOwn(point,"toString");        // false -- tốt
Object.hasOwn(point,"getDoubleX");      // true -- ôi không :(
```

Bạn thấy rồi chứ? Bằng cách định nghĩa một giá trị hàm và gắn nó như một thuộc tính trường/thành viên, chúng ta đang đánh mất tính chất phương thức nguyên mẫu được chia sẻ của hàm, và nó trở thành giống như bất kỳ thuộc tính trên mỗi thể hiện nào. Điều đó có nghĩa là chúng ta đang tạo một thuộc tính hàm mới **cho mỗi thể hiện**, thay vì nó được tạo chỉ một lần trên `prototype` của constructor lớp.

Điều đó gây lãng phí về hiệu suất và bộ nhớ, ngay cả khi chỉ một chút. Chỉ riêng điều đó đã đủ để tránh nó.

Nhưng tôi sẽ tranh luận rằng quan trọng hơn nhiều, những gì bạn đã làm với mẫu này là vô hiệu hóa chính lý do tại sao việc sử dụng `class` và các phương thức nhận biết `this` lại hữu ích/mạnh mẽ!

Nếu bạn phải trải qua tất cả những rắc rối để định nghĩa các phương thức lớp với các tham chiếu `this.` xuyên suốt chúng, nhưng sau đó bạn khóa/ràng buộc hầu hết hoặc tất cả các phương thức đó vào một thể hiện đối tượng cụ thể, về cơ bản bạn đã đi vòng quanh thế giới chỉ để sang nhà hàng xóm.

Nếu tất cả những gì bạn muốn là (các) hàm được cố định tĩnh vào một "ngữ cảnh" cụ thể, và không cần bất kỳ sự năng động hay chia sẻ nào, thì thứ bạn muốn là... **closure**. Và bạn thật may mắn: Tôi đã viết cả một cuốn sách trong bộ này ("Phạm vi & Closures") về cách sử dụng closure để các hàm ghi nhớ/truy cập phạm vi được định nghĩa tĩnh của chúng (hay còn gọi là "ngữ cảnh"). Đó là một cách tiếp cận phù hợp hơn nhiều, và đơn giản hơn để viết mã, để có được những gì bạn đang theo đuổi.

Đừng lạm dụng/sử dụng sai `class` và biến nó thành một bộ sưu tập closure được ca tụng quá mức, hào nhoáng.

Để rõ ràng, tôi *không* nói rằng: không bao giờ sử dụng các hàm mũi tên `=>` bên trong các lớp.

Tôi *đang* nói rằng: không bao giờ gắn một hàm mũi tên `=>` như một thuộc tính thể hiện thay cho một phương thức lớp nguyên mẫu động, hoặc do thói quen vô thức, hoặc lười biếng gõ ít ký tự hơn, hoặc sự tiện lợi ràng buộc `this` sai lầm.

Trong một chương tiếp theo, chúng ta sẽ đi sâu vào cách hiểu và tận dụng đúng cách sức mạnh đầy đủ của cơ chế `this` động.

## Mở rộng Lớp (Class Extension)

Cách để mở khóa sức mạnh của kế thừa lớp là thông qua từ khóa `extends`, định nghĩa mối quan hệ giữa hai lớp:

```js
class Point2d {
    x = 3
    y = 4

    getX() {
        return this.x;
    }
}

class Point3d extends Point2d {
    x = 21
    y = 10
    z = 5

    printDoubleX() {
        console.log(`double x: ${this.getX() * 2}`);
    }
}

var point = new Point2d();

point.getX();                   // 3

var anotherPoint = new Point3d();

anotherPoint.getX();            // 21
anotherPoint.printDoubleX();    // double x: 42
```

Hãy dành một chút thời gian để đọc lại đoạn mã đó và đảm bảo bạn hiểu đầy đủ những gì đang xảy ra.

Lớp cơ sở `Point2d` định nghĩa các trường (thành viên) được gọi là `x` và `y`, và cung cấp cho chúng các giá trị ban đầu tương ứng là `3` và `4`. Nó cũng định nghĩa một phương thức `getX()` truy cập thành viên thể hiện `x` này và trả về nó. Chúng ta thấy hành vi đó được minh họa trong lệnh gọi phương thức `point.getX()`.

Nhưng lớp `Point3d` mở rộng `Point2d`, làm cho `Point3d` trở thành một lớp dẫn xuất (derived-class), lớp con (child-class), hoặc (phổ biến nhất) subclass. Trong `Point3d`, cùng một thuộc tính `x` được kế thừa từ `Point2d` được khởi tạo lại với một giá trị `21` khác, cũng như `y` bị ghi đè thành giá trị từ `4`, thành `10`.

Nó cũng thêm một phương thức trường/thành viên `z` mới, cũng như một phương thức `printDoubleX()`, bản thân nó gọi `this.getX()`.

Khi `anotherPoint.printDoubleX()` được gọi, `this.getX()` được kế thừa do đó được gọi, và phương thức đó tham chiếu đến `this.x`. Vì `this` đang trỏ vào thể hiện lớp (hay còn gọi là `anotherPoint`), giá trị nó tìm thấy bây giờ là `21` (thay vì `3` từ thành viên `x` của đối tượng `point`).

### Mở rộng Biểu thức

// TODO: đề cập `class Foo extends ..` trong đó `..` là một biểu thức, không phải tên lớp

### Ghi đè Phương thức

Ngoài việc ghi đè một trường/thành viên trong một lớp con, bạn cũng có thể ghi đè (định nghĩa lại) một phương thức:

```js
class Point2d {
    x = 3
    y = 4

    getX() {
        return this.x;
    }
}

class Point3d extends Point2d {
    x = 21
    y = 10
    z = 5

    getX() {
        return this.x * 2;
    }
    printX() {
        console.log(`double x: ${this.getX()}`);
    }
}

var point = new Point3d();

point.printX();       // double x: 42
```

Lớp con `Point3d` ghi đè phương thức `getX()` được kế thừa để cung cấp cho nó hành vi khác. Tuy nhiên, bạn vẫn có thể khởi tạo lớp cơ sở `Point2d`, lớp này sau đó sẽ cung cấp một đối tượng sử dụng định nghĩa gốc (`return this.x;`) cho `getX()`.

Nếu bạn muốn truy cập một phương thức được kế thừa từ một lớp con ngay cả khi nó đã bị ghi đè, bạn có thể sử dụng `super` thay vì `this`:

```js
class Point2d {
    x = 3
    y = 4

    getX() {
        return this.x;
    }
}

class Point3d extends Point2d {
    x = 21
    y = 10
    z = 5

    getX() {
        return this.x * 2;
    }
    printX() {
        console.log(`x: ${super.getX()}`);
    }
}

var point = new Point3d();

point.printX();       // x: 21
```

Khả năng các phương thức cùng tên, ở các cấp độ khác nhau của hệ thống phân cấp kế thừa, thể hiện hành vi khác nhau khi được truy cập trực tiếp hoặc tương đối với `super`, được gọi là *đa hình phương thức* (method polymorphism). Đó là một phần rất mạnh mẽ của định hướng lớp, khi được sử dụng một cách thích hợp.

### Thật Siêu! (That's Super!)

Ngoài việc một phương thức lớp con truy cập một định nghĩa phương thức được kế thừa (ngay cả khi bị ghi đè trên lớp con) thông qua tham chiếu `super.`, một constructor lớp con phải gọi thủ công constructor lớp cơ sở được kế thừa thông qua lệnh gọi hàm `super(..)`:

```js
class Point2d {
    x
    y
    constructor(x,y) {
        this.x = x;
        this.y = y;
    }
}

```js
class Point3d extends Point2d {
    z
    constructor(x,y,z) {
        super(x,y);
        this.z = z;
    }
    toString() {
        console.log(`(${this.x},${this.y},${this.z})`);
    }
}

var point = new Point3d(3,4,5);

point.toString();       // (3,4,5)
```

| CẢNH BÁO: |
| :--- |
| Một constructor lớp con được định nghĩa rõ ràng *phải* gọi `super(..)` để chạy khởi tạo của lớp được kế thừa, và điều đó phải xảy ra trước khi constructor lớp con thực hiện bất kỳ tham chiếu nào đến `this` hoặc kết thúc/trả về. Nếu không, một ngoại lệ thời gian chạy sẽ được ném ra khi constructor lớp con đó được gọi (thông qua `new`). Nếu bạn bỏ qua constructor lớp con, constructor mặc định sẽ tự động -- rất may! -- gọi `super()` cho bạn. |

Một sắc thái cần lưu ý: nếu bạn định nghĩa một trường (công khai hoặc riêng tư) bên trong một lớp con, và định nghĩa rõ ràng một `constructor(..)` cho lớp con này, các khởi tạo trường sẽ được xử lý không phải ở đầu constructor, mà *ở giữa* lệnh gọi `super(..)` và bất kỳ mã nào tiếp theo trong constructor.

Hãy chú ý kỹ đến thứ tự của các thông báo console ở đây:

```js
class Point2d {
    x
    y
    constructor(x,y) {
        console.log("Đang chạy constructor Point2d(..)");
        this.x = x;
        this.y = y;
    }
}

class Point3d extends Point2d {
    z = console.log("Đang khởi tạo trường 'z'")

    constructor(x,y,z) {
        console.log("Đang chạy constructor Point3d(..)");
        super(x,y);

        console.log(`Đang đặt thuộc tính thể hiện 'z' thành ${z}`);
        this.z = z;
    }
    toString() {
        console.log(`(${this.x},${this.y},${this.z})`);
    }
}

var point = new Point3d(3,4,5);
// Đang chạy constructor Point3d(..)
// Đang chạy constructor Point2d(..)
// Đang khởi tạo trường 'z'
// Đang đặt thuộc tính thể hiện 'z' thành 5
```

Như các thông báo console minh họa, việc khởi tạo trường `z = ..` xảy ra *ngay sau* lệnh gọi `super(x,y)`, *trước khi* ``console.log(`Đang đặt thuộc tính thể hiện...`)`` được thực thi. Có lẽ hãy nghĩ về nó giống như các khởi tạo trường được gắn vào cuối lệnh gọi `super(..)`, vì vậy chúng chạy trước khi bất kỳ điều gì khác trong constructor thực hiện.

#### Lớp Nào?

Bạn có thể cần xác định trong một constructor xem lớp đó đang được khởi tạo trực tiếp, hay đang được khởi tạo từ một lớp con với một lệnh gọi `super()`. Chúng ta có thể sử dụng một "thuộc tính giả" đặc biệt `new.target`:

```js
class Point2d {
    // ..

    constructor(x,y) {
        if (new.target === Point2d) {
            console.log("Đang xây dựng thể hiện 'Point2d'");
        }
    }

    // ..
}

class Point3d extends Point2d {
    // ..

    constructor(x,y,z) {
        super(x,y);

        if (new.target === Point3d) {
            console.log("Đang xây dựng thể hiện 'Point3d'");
        }
    }

    // ..
}

var point = new Point2d(3,4);
// Đang xây dựng thể hiện 'Point2d'

var anotherPoint = new Point3d(3,4,5);
// Đang xây dựng thể hiện 'Point3d'
```

### Nhưng Loại Thể Hiện Nào?

Bạn có thể muốn xem xét nội bộ một thể hiện đối tượng nhất định để xem liệu nó có phải là một thể hiện của một lớp cụ thể hay không. Chúng ta làm điều này với toán tử `instanceof`:

```js
class Point2d { /* .. */ }
class Point3d extends Point2d { /* .. */ }

var point = new Point2d(3,4);

point instanceof Point2d;           // true
point instanceof Point3d;           // false

var anotherPoint = new Point3d(3,4,5);

anotherPoint instanceof Point2d;    // true
anotherPoint instanceof Point3d;    // true
```

Có vẻ lạ khi thấy `anotherPoint instanceof Point2d` trả về `true`. Để hiểu rõ hơn tại sao, có lẽ hữu ích khi hình dung cả hai chuỗi `[[Prototype]]`:

```
Point2d.prototype
        /       \
       /         \
      /           \
  point   Point3d.prototype
                    \
                     \
                      \
                    anotherPoint
```

Toán tử `instanceof` không chỉ nhìn vào đối tượng hiện tại, mà còn duyệt qua toàn bộ hệ thống phân cấp kế thừa lớp (chuỗi `[[Prototype]]`) cho đến khi nó tìm thấy một kết quả khớp. Do đó, `anotherPoint` là một thể hiện của cả `Point3d` và `Point2d`.

Để minh họa sự thật này rõ ràng hơn một chút, một cách khác (kém tiện dụng hơn) để thực hiện cùng loại kiểm tra như `instanceof` là với tiện ích (được kế thừa từ `Object.prototype`) `isPrototypeOf(..)`:

```js
Point2d.prototype.isPrototypeOf(point);             // true
Point3d.prototype.isPrototypeOf(point);             // false

Point2d.prototype.isPrototypeOf(anotherPoint);      // true
Point3d.prototype.isPrototypeOf(anotherPoint);      // true
```

Tiện ích này làm cho rõ ràng hơn một chút tại sao cả `Point2d.prototype.isPrototypeOf(anotherPoint)` và `anotherPoint instanceof Point2d` đều trả về `true`: đối tượng `Point2d.prototype` *nằm* trong chuỗi `[[Prototype]]` của `anotherPoint`.

Nếu thay vào đó bạn muốn kiểm tra xem thể hiện đối tượng có được tạo *chỉ và trực tiếp* bởi một lớp nhất định hay không, hãy kiểm tra thuộc tính `constructor` của thể hiện.

```js
point.constructor === Point2d;          // true
point.constructor === Point3d;          // false

anotherPoint.constructor === Point2d;   // false
anotherPoint.constructor === Point3d;   // true
```

| LƯU Ý: |
| :--- |
| Thuộc tính `constructor` được hiển thị ở đây *không* thực sự hiện diện trên (được sở hữu bởi) các đối tượng thể hiện `point` hoặc `anotherPoint`. Vậy nó đến từ đâu!? Nó nằm trên đối tượng nguyên mẫu được liên kết `[[Prototype]]` của mỗi đối tượng: `Point2d.prototype.constructor === Point2d` và `Point3d.prototype.constructor === Point3d`. |

### "Kế thừa" Là Chia sẻ, Không phải Sao chép

Có vẻ như `Point3d`, khi nó `extends` lớp `Point2d`, về cơ bản đang nhận được một *bản sao* của tất cả hành vi được định nghĩa trong `Point2d`. Hơn nữa, có vẻ như thể hiện đối tượng cụ thể `anotherPoint` nhận được, *được sao chép xuống* nó, tất cả các phương thức từ `Point3d` (và mở rộng ra, cũng từ `Point2d`).

Tuy nhiên, đó không phải là mô hình tư duy chính xác để sử dụng cho việc triển khai định hướng lớp của JS. Hãy nhớ lại định nghĩa lớp cơ sở và lớp con này, cũng như việc khởi tạo `anotherPoint`:

```js
class Point2d {
    x
    y
    constructor(x,y) {
        this.x = x;
        this.y = y;
    }
}

class Point3d extends Point2d {
    z
    constructor(x,y,z) {
        super(x,y);
        this.z = z;
    }
    toString() {
        console.log(`(${this.x},${this.y},${this.z})`);
    }
}

var anotherPoint = new Point3d(3,4,5);
```

Nếu bạn kiểm tra đối tượng `anotherPoint`, bạn sẽ thấy nó chỉ có các thuộc tính `x`, `y`, và `z` (thành viên thể hiện) trên nó, nhưng không có phương thức `toString()`:

```js
Object.hasOwn(anotherPoint,"x");                       // true
Object.hasOwn(anotherPoint,"y");                       // true
Object.hasOwn(anotherPoint,"z");                       // true

Object.hasOwn(anotherPoint,"toString");                // false
```

Phương thức `toString()` đó nằm ở đâu? Trên đối tượng nguyên mẫu:

```js
Object.hasOwn(Point3d.prototype,"toString");    // true
```

Và `anotherPoint` có quyền truy cập vào phương thức đó thông qua liên kết `[[Prototype]]` của nó (xem Chương 2). Nói cách khác, các đối tượng nguyên mẫu **chia sẻ quyền truy cập** vào (các) phương thức của chúng với (các) lớp con và (các) thể hiện. (Các) phương thức vẫn ở tại chỗ, và không được sao chép xuống chuỗi kế thừa.

Mặc dù cú pháp `class` rất hay, đừng quên những gì thực sự đang xảy ra bên dưới cú pháp: JS *chỉ* đang nối các đối tượng với nhau dọc theo một chuỗi `[[Prototype]]`.

## Hành vi Lớp Tĩnh (Static Class Behavior)

Cho đến nay, chúng ta đã nhấn mạnh hai vị trí khác nhau để dữ liệu hoặc hành vi (phương thức) cư trú: trên nguyên mẫu của constructor, hoặc trên thể hiện. Nhưng có một lựa chọn thứ ba: trên chính constructor (đối tượng hàm).

Trong một hệ thống hướng lớp truyền thống, các phương thức được định nghĩa trên một lớp không phải là những thứ cụ thể mà bạn có thể gọi hoặc tương tác. Bạn phải khởi tạo một lớp để có một đối tượng cụ thể để gọi các phương thức đó. Các ngôn ngữ nguyên mẫu như JS làm mờ ranh giới này một chút: tất cả các phương thức được định nghĩa trong lớp là các hàm "thực" cư trú trên nguyên mẫu của constructor, và do đó bạn có thể gọi chúng. Nhưng như tôi đã khẳng định trước đó, bạn thực sự *không nên* làm như vậy, vì đây không phải là cách JS giả định bạn sẽ viết các `class` của mình, và có một số hành vi trường hợp góc kỳ lạ mà bạn có thể gặp phải. Tốt nhất là hãy đi trên con đường hẹp mà `class` vạch ra cho bạn.

Không phải tất cả hành vi mà chúng ta định nghĩa và muốn liên kết/tổ chức với một lớp *cần* phải nhận biết về một thể hiện. Hơn nữa, đôi khi một lớp cần định nghĩa công khai dữ liệu (như hằng số) mà các nhà phát triển sử dụng lớp đó cần truy cập, độc lập với bất kỳ thể hiện nào họ có thể đã tạo hoặc chưa tạo.

Vậy, làm thế nào để một hệ thống lớp cho phép định nghĩa dữ liệu và hành vi như vậy nên có sẵn với một lớp nhưng độc lập với (không nhận biết về) các đối tượng được khởi tạo? **Các thuộc tính và hàm tĩnh** (Static properties and functions).

| LƯU Ý: |
| :--- |
| Tôi sẽ sử dụng "thuộc tính tĩnh" / "hàm tĩnh", thay vì "thành viên" / "phương thức", chỉ để rõ ràng hơn rằng có sự phân biệt giữa các thành viên bị ràng buộc với thể hiện / phương thức nhận biết thể hiện, và các thuộc tính không phải thể hiện và các hàm không nhận biết thể hiện. |

Chúng ta sử dụng từ khóa `static` trong thân `class` của mình để phân biệt các định nghĩa này:

```js
class Point2d {
    // class statics
    static origin = new Point2d(0,0)
    static distance(point1,point2) {
        return Math.sqrt(
            ((point2.x - point1.x) ** 2) +
            ((point2.y - point1.y) ** 2)
        );
    }

    // instance members and methods
    x
    y
    constructor(x,y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return `(${this.x},${this.y})`;
    }
}

console.log(`Điểm bắt đầu: ${Point2d.origin}`);
// Điểm bắt đầu: (0,0)

var next = new Point2d(3,4);
console.log(`Điểm tiếp theo: ${next}`);
// Điểm tiếp theo: (3,4)

console.log(`Khoảng cách: ${
    Point2d.distance( Point2d.origin, next )
}`);
// Khoảng cách: 5
```

`Point2d.origin` là một thuộc tính tĩnh, tình cờ giữ một thể hiện đã được xây dựng của lớp chúng ta. Và `Point2d.distance(..)` là một hàm tĩnh tính toán khoảng cách Descartes 2 chiều giữa hai điểm.

Tất nhiên, chúng ta có thể đã đặt hai thứ này ở đâu đó khác ngoài việc là `static` trên định nghĩa lớp. Nhưng vì chúng liên quan trực tiếp đến lớp `Point2d`, nên *hợp lý nhất* là tổ chức chúng ở đó.

| LƯU Ý: |
| :--- |
| Đừng quên rằng khi bạn sử dụng cú pháp `class`, tên `Point2d` thực sự là tên của một hàm constructor mà JS định nghĩa. Vì vậy `Point2d.origin` chỉ là một truy cập thuộc tính thông thường trên đối tượng hàm đó. Đó là những gì tôi muốn nói ở đầu phần này khi tôi đề cập đến một vị trí thứ ba để lưu trữ *những thứ* liên quan đến các lớp; trong JS, các `static` được lưu trữ dưới dạng các thuộc tính trên hàm constructor. Hãy cẩn thận để không nhầm lẫn những thứ đó với các thuộc tính được lưu trữ trên `prototype` của constructor (phương thức) và các thuộc tính được lưu trữ trên thể hiện (thành viên). |

### Khởi tạo Thuộc tính Tĩnh

Giá trị trong một khởi tạo tĩnh (`static whatever = ..`) có thể bao gồm các tham chiếu `this`, tham chiếu đến chính lớp (thực ra là constructor) thay vì đến một thể hiện:

```js
class Point2d {
    // class statics
    static originX = 0
    static originY = 0
    static origin = new this(this.originX,this.originY)

    // ..
}
```

| CẢNH BÁO: |
| :--- |
| Tôi không khuyên bạn thực sự làm thủ thuật `new this(..)` mà tôi đã minh họa ở đây. Đó chỉ là cho mục đích minh họa. Mã sẽ dễ đọc hơn với `new Point2d(this.originX,this.originY)`, vì vậy hãy ưu tiên cách tiếp cận đó. |

Một chi tiết quan trọng không nên bỏ qua: không giống như các khởi tạo trường công khai, chỉ xảy ra khi một khởi tạo (với `new`) xảy ra, các khởi tạo tĩnh của lớp luôn chạy *ngay lập tức* sau khi `class` đã được định nghĩa. Hơn nữa, thứ tự của các khởi tạo tĩnh rất quan trọng; bạn có thể nghĩ về các câu lệnh như thể chúng đang được đánh giá từng cái một.

Cũng giống như các thành viên lớp, các thuộc tính tĩnh không nhất thiết phải được khởi tạo (mặc định: `undefined`), nhưng việc làm như vậy phổ biến hơn nhiều. Không có nhiều tiện ích trong việc khai báo một thuộc tính tĩnh không có giá trị khởi tạo (`static whatever`); Truy cập `Point2d.whatever` hoặc `Point2d.nonExistent` đều sẽ dẫn đến `undefined`.

Gần đây (trong ES2022), từ khóa `static` đã được mở rộng để nó hiện có thể định nghĩa một khối bên trong thân `class` để khởi tạo phức tạp hơn cho các `static`:

```js
class Point2d {
    // class statics
    static origin = new Point2d(0,0)
    static distance(point1,point2) {
        return Math.sqrt(
            ((point2.x - point1.x) ** 2) +
            ((point2.y - point1.y) ** 2)
        );
    }

    // khối khởi tạo tĩnh (kể từ ES2022)
    static {
        let outerPoint = new Point2d(6,8);
        this.maxDistance = this.distance(
            this.origin,
            outerPoint
        );
    }

    // ..
}

Point2d.maxDistance;        // 10
```

`let outerPoint = ..` ở đây không phải là một tính năng `class` đặc biệt; nó chính xác giống như một khai báo `let` bình thường trong bất kỳ khối phạm vi bình thường nào (xem cuốn sách "Phạm vi & Closures" của bộ này). Chúng ta chỉ đơn thuần khai báo một thể hiện cục bộ của `Point2d` được gán cho `outerPoint`, sau đó sử dụng giá trị đó để lấy phép gán cho thuộc tính tĩnh `maxDistance`.

Các khối khởi tạo tĩnh cũng hữu ích cho những thứ như câu lệnh `try..catch` xung quanh các tính toán biểu thức.

### Kế thừa Tĩnh (Static Inheritance)

Các static của lớp được kế thừa bởi các lớp con (rõ ràng, là các static!), có thể bị ghi đè, và `super` có thể được sử dụng cho các tham chiếu lớp cơ sở (và đa hình hàm tĩnh), tất cả theo cùng một cách như kế thừa hoạt động với các thành viên/phương thức thể hiện:

```js
class Point2d {
    static origin = /* .. */
    static distance(x,y) { /* .. */ }

    static {
        // ..
        this.maxDistance = /* .. */;
    }

    // ..
}

class Point3d extends Point2d {
    // class statics
    static origin = new Point3d(
        // ở đây, các tham chiếu `this.origin` sẽ không
        // hoạt động (tự tham chiếu), vì vậy chúng ta sử dụng
        // các tham chiếu `super.origin` thay thế
        super.origin.x, super.origin.y, 0
    )
    static distance(point1,point2) {
        // ở đây, super.distance(..) là Point2d.distance(..),
        // nếu chúng ta cần gọi nó

        return Math.sqrt(
            ((point2.x - point1.x) ** 2) +
            ((point2.y - point1.y) ** 2) +
            ((point2.z - point1.z) ** 2)
        );
    }

    // các thành viên/phương thức thể hiện
    z
    constructor(x,y,z) {
        super(x,y);     // <-- đừng quên dòng này!
        this.z = z;
    }
    toString() {
        return `(${this.x},${this.y},${this.z})`;
    }
}

Point2d.maxDistance;        // 10
Point3d.maxDistance;        // 10
```

Như bạn có thể thấy, thuộc tính tĩnh `maxDistance` mà chúng ta đã định nghĩa trên `Point2d` đã được kế thừa dưới dạng một thuộc tính tĩnh trên `Point3d`.

| MẸO: |
| :--- |
| Hãy nhớ: bất cứ khi nào bạn định nghĩa một constructor lớp con, bạn sẽ cần gọi `super(..)` trong đó, thường là câu lệnh đầu tiên. Tôi thấy điều đó quá dễ quên. |

Đừng bỏ qua hành vi JS cơ bản ở đây. Cũng giống như kế thừa phương thức đã thảo luận trước đó, "kế thừa" tĩnh *không* phải là sao chép các thuộc tính/hàm tĩnh này từ lớp cơ sở sang lớp con; đó là chia sẻ thông qua chuỗi `[[Prototype]]`. Cụ thể, hàm constructor `Point3d()` có liên kết `[[Prototype]]` của nó được thay đổi bởi JS (từ mặc định là `Function.prototype`) thành `Point2d`, điều này cho phép `Point3d.maxDistance` ủy quyền cho `Point2d.maxDistance`.

Cũng thú vị, có lẽ chỉ về mặt lịch sử bây giờ, khi lưu ý rằng kế thừa tĩnh -- vốn là một phần của bộ tính năng cơ chế `class` ES6 ban đầu! -- là một tính năng cụ thể đã vượt ra ngoài "chỉ là đường cú pháp". Kế thừa tĩnh, như chúng ta thấy nó được minh họa ở đây, đã *không* thể đạt được/mô phỏng trong JS trước ES6, trong kiểu mã lớp nguyên mẫu cũ. Đó là một hành vi mới đặc biệt chỉ được giới thiệu kể từ ES6.

## Hành vi Lớp Riêng tư (Private Class Behavior)

Mọi thứ chúng ta đã thảo luận cho đến nay như một phần của định nghĩa `class` đều hiển thị/truy cập công khai, hoặc dưới dạng các thuộc tính/hàm tĩnh trên lớp, các phương thức trên `prototype` của constructor, hoặc các thuộc tính thành viên trên thể hiện.

Nhưng làm thế nào để bạn lưu trữ thông tin không thể nhìn thấy từ bên ngoài lớp? Đây là một trong những tính năng được yêu cầu nhiều nhất, và là những phàn nàn lớn nhất với `class` của JS, cho đến khi nó cuối cùng được giải quyết trong ES2022.

`class` hiện hỗ trợ cú pháp mới để khai báo các trường riêng tư (thành viên thể hiện) và các phương thức riêng tư. Ngoài ra, các thuộc tính/hàm tĩnh riêng tư cũng có thể thực hiện được.

### Động lực?

Trước khi chúng ta minh họa cách thực hiện các private của `class`, cần suy ngẫm tại sao đây là một tính năng hữu ích?

Với các mẫu thiết kế hướng closure (một lần nữa, xem cuốn sách "Phạm vi & Closures" của bộ này), chúng ta tự động có được "sự riêng tư" được tích hợp sẵn. Khi bạn khai báo một biến bên trong một phạm vi, nó không thể được nhìn thấy bên ngoài phạm vi đó. Chấm hết. Giảm khả năng hiển thị phạm vi của một khai báo rất hữu ích trong việc ngăn chặn xung đột không gian tên (tên biến giống hệt nhau).

Nhưng điều quan trọng hơn nữa là đảm bảo thiết kế phần mềm "phòng thủ" đúng đắn, cái gọi là "Nguyên tắc Đặc quyền Tối thiểu" (Principle of Least Privilege - POLP) [^POLP]. POLP nói rằng chúng ta chỉ nên để lộ một phần thông tin hoặc khả năng trong phần mềm của mình cho diện tích bề mặt nhỏ nhất cần thiết.

Việc phơi bày quá mức mở phần mềm của chúng ta ra một số vấn đề làm phức tạp bảo mật/bảo trì phần mềm, bao gồm một đoạn mã khác hành động ác ý để làm điều gì đó mà mã của chúng ta không mong đợi hoặc dự định. Hơn nữa, có mối quan tâm ít quan trọng hơn nhưng vẫn có vấn đề tương tự về các phần khác của phần mềm của chúng ta dựa vào (sử dụng) các phần mã của chúng ta mà lẽ ra chúng ta nên dành riêng làm chi tiết triển khai ẩn. Một khi mã khác dựa vào các chi tiết triển khai của mã của chúng ta, chúng ta ngừng có thể tái cấu trúc mã của mình mà không có khả năng phá vỡ các phần khác của chương trình.

Vì vậy, tóm lại, chúng ta *nên* ẩn các chi tiết triển khai nếu chúng không cần thiết phải được phơi bày. Theo nghĩa này, hệ thống `class` của JS cảm thấy hơi quá dễ dãi ở chỗ mọi thứ mặc định là công khai. Các tính năng riêng tư của lớp là một bổ sung đáng hoan nghênh cho thiết kế phần mềm đúng đắn hơn.

#### Quá Riêng tư?

Tất cả những điều đó đã nói, tôi phải ném một chút nước lạnh vào bữa tiệc riêng tư của lớp.

Tôi đã đề nghị mạnh mẽ rằng bạn chỉ nên sử dụng `class` nếu bạn thực sự sẽ tận dụng hầu hết hoặc tất cả những gì định hướng lớp mang lại cho bạn. Nếu không, bạn sẽ phù hợp hơn khi sử dụng các tính năng trụ cột cốt lõi khác của JS để tổ chức mã, chẳng hạn như với mẫu closure.

Một trong những khía cạnh quan trọng nhất của định hướng lớp là kế thừa lớp con, như chúng ta đã thấy được minh họa nhiều lần cho đến nay trong chương này. Đoán xem điều gì xảy ra với một thành viên/phương thức riêng tư trong một lớp cơ sở, khi nó được mở rộng bởi một lớp con?

Các thành viên/phương thức riêng tư là riêng tư **chỉ đối với lớp chúng được định nghĩa trong đó**, và **không** được kế thừa theo bất kỳ cách nào bởi một lớp con. Ồ ồ.

Điều đó có vẻ không phải là một mối quan tâm quá lớn, cho đến khi bạn bắt đầu làm việc với `class` và các thành viên/phương thức riêng tư trong phần mềm thực tế. Bạn có thể nhanh chóng gặp phải tình huống mà bạn cần truy cập một phương thức riêng tư, hoặc thường xuyên hơn thậm chí, chỉ là một thành viên riêng tư, từ lớp con, để lớp con có thể mở rộng/tăng cường hành vi của lớp cơ sở như mong muốn. Và bạn có thể hét lên trong sự thất vọng khá nhanh chóng khi bạn nhận ra điều này là không thể.

Điều gì xảy ra tiếp theo chắc chắn là một quyết định khó xử: bạn có quay lại làm cho nó công khai, để lớp con có thể truy cập nó không? Hừm. Hoặc, tệ hơn, bạn có cố gắng thiết kế lại lớp cơ sở để bóp méo thiết kế của các thành viên/phương thức của nó, sao cho việc thiếu quyền truy cập được giải quyết một phần. Điều đó thường liên quan đến việc tham số hóa quá mức gây kiệt sức (với các private làm giá trị tham số mặc định) của các phương thức, và các thủ thuật khác như vậy. Hừm gấp đôi.

Thành thật mà nói, không có câu trả lời nào đặc biệt tuyệt vời ở đây. Nếu bạn có kinh nghiệm với định hướng lớp trong các ngôn ngữ lớp truyền thống hơn như Java hoặc C++, bạn có thể nghi ngờ về lý do tại sao chúng ta không có khả năng hiển thị *protected* (được bảo vệ) ở giữa *public* và *private*. Đó chính xác là những gì *protected* dành cho: giữ một cái gì đó riêng tư đối với một lớp VÀ bất kỳ lớp con nào của nó. Những ngôn ngữ đó cũng có các tính năng *friend*, nhưng điều đó nằm ngoài phạm vi thảo luận của chúng ta ở đây.

Đáng buồn thay, không chỉ JS không có khả năng hiển thị *protected*, có vẻ như (ngay cả khi nó hữu ích như vậy!) nó khó có thể là một tính năng JS. Nó đã được thảo luận rất chi tiết trong hơn một thập kỷ (trước khi ES6 thậm chí là một thứ), và đã có nhiều đề xuất cho nó.

Tôi không nên nói nó sẽ *không bao giờ* xảy ra, bởi vì đó không phải là nền tảng vững chắc để đặt cược trong bất kỳ phần mềm nào. Nhưng rất khó xảy ra, bởi vì nó thực sự phản bội chính trụ cột mà `class` được xây dựng trên đó. Nếu bạn tò mò, hoặc (có khả năng hơn) chắc chắn rằng *phải có một cách*, tôi sẽ đề cập đến sự không tương thích của khả năng hiển thị *protected* trong các cơ chế của JS trong một phụ lục.

Điểm mấu chốt ở đây là, tính đến thời điểm hiện tại, JS không có khả năng hiển thị *protected*, và nó sẽ không có sớm. Và khả năng hiển thị *protected* thực sự, trong thực tế, hữu ích hơn nhiều so với khả năng hiển thị *private*.

Vì vậy, chúng ta quay lại câu hỏi: **Tại sao bạn nên quan tâm đến việc làm cho bất kỳ nội dung `class` nào trở nên riêng tư?**

Nếu tôi thành thật: có lẽ bạn không nên. Hoặc có lẽ bạn nên. Điều đó tùy thuộc vào bạn. Chỉ cần đi vào nó với nhận thức về những trở ngại.

### Thành viên/Phương thức Riêng tư

Bạn hào hứng khi cuối cùng cũng thấy cú pháp cho khả năng hiển thị *private* kỳ diệu, phải không? Xin đừng bắn người đưa tin nếu bạn cảm thấy tức giận hoặc buồn bã trước những gì bạn sắp thấy.

```js
class Point2d {
    // statics
    static samePoint(point1,point2) {
        return point1.#ID === point2.#ID;
    }

    // privates
    #ID = null
    #assignID() {
        this.#ID = Math.round(Math.random() * 1e9);
    }

    // publics
    x
    y
    constructor(x,y) {
        this.#assignID();
        this.x = x;
        this.y = y;
    }
}

var one = new Point2d(3,4);
var two = new Point2d(3,4);

Point2d.samePoint(one,two);         // false
Point2d.samePoint(one,one);         // true
```
```

Không, JS đã không làm điều hợp lý và giới thiệu một từ khóa `private` như họ đã làm với `static`. Thay vào đó, họ đã giới thiệu `#`. (chèn trò đùa nhạt nhẽo về việc thế hệ thiên niên kỷ trên mạng xã hội yêu thích hashtag, hoặc đại loại thế)

| MẸO: |
| :--- |
| Và vâng, có một triệu lẻ một cuộc thảo luận về lý do tại sao không. Tôi có thể dành nhiều chương để kể lại toàn bộ lịch sử, nhưng thành thật mà nói tôi không quan tâm. Tôi nghĩ cú pháp này xấu xí, và nhiều người khác cũng vậy. Và một số người thích nó! Nếu bạn ở trong trại sau, mặc dù tôi hiếm khi làm điều gì đó như thế này, tôi chỉ định nói: **hãy chấp nhận nó**. Đã quá muộn cho bất kỳ cuộc tranh luận hay cầu xin nào nữa. |

Cú pháp `#whatever` (bao gồm dạng `this.#whatever`) chỉ hợp lệ bên trong thân `class`. Nó sẽ ném ra lỗi cú pháp nếu được sử dụng bên ngoài một `class`.

Không giống như các trường công khai/thành viên thể hiện, các trường riêng tư/thành viên thể hiện *phải* được khai báo trong thân `class`. Bạn không thể thêm một thành viên riêng tư vào một khai báo lớp một cách linh hoạt trong khi ở trong phương thức constructor; các phép gán kiểu `this.#whatever = ..` chỉ hoạt động nếu trường riêng tư `#whatever` được khai báo trong thân lớp. Hơn nữa, mặc dù các trường riêng tư có thể được gán lại, chúng không thể bị `delete` (xóa) khỏi một thể hiện, theo cách mà một trường công khai/thành viên lớp có thể.

#### Lớp Con + Riêng tư

Tôi đã cảnh báo trước đó rằng việc phân lớp với các lớp có các thành viên/phương thức riêng tư có thể là một cái bẫy hạn chế. Nhưng điều đó không có nghĩa là chúng không thể được sử dụng cùng nhau.

Vì "kế thừa" trong JS là chia sẻ (thông qua chuỗi `[[Prototype]]`), nếu bạn gọi một phương thức được kế thừa trong một lớp con, và phương thức được kế thừa đó lần lượt truy cập/gọi các private trong lớp chủ (cơ sở) của nó, điều này hoạt động tốt:

```js
class Point2d { /* .. */ }

class Point3d extends Point2d {
    z
    constructor(x,y,z) {
        super(x,y);
        this.z = z;
    }
}

var one = new Point3d(3,4,5);
```

Lệnh gọi `super(x,y)` trong constructor này gọi constructor lớp cơ sở được kế thừa (`Point2d(..)`), bản thân nó truy cập phương thức riêng tư `#assignID()` của `Point2d` (xem đoạn mã trước đó). Không có ngoại lệ nào được ném ra, mặc dù `Point3d` không thể trực tiếp nhìn thấy hoặc truy cập các private `#ID` / `#assignID()` thực sự được lưu trữ trên thể hiện (được đặt tên là `one` ở đây).

Trên thực tế, ngay cả hàm `static samePoint(..)` được kế thừa cũng sẽ hoạt động từ `Point3d` hoặc `Point2d`:

```js
Point2d.samePoint(one,one);         // true
Point3d.samePoint(one,one);         // true
```

Thực ra, điều đó không nên quá ngạc nhiên, vì:

```js
Point2d.samePoint === Point3d.samePoint;
```

Tham chiếu hàm được kế thừa là *chính xác cùng một hàm* như tham chiếu hàm cơ sở; nó không phải là một bản sao của hàm. Vì hàm đang được đề cập không có tham chiếu `this` trong đó, bất kể nó được gọi từ đâu, nó sẽ tạo ra cùng một kết quả.

Tuy nhiên, vẫn thật đáng tiếc khi `Point3d` không có cách nào để truy cập/ảnh hưởng, hoặc thậm chí biết về, các private `#ID` / `#assignID()` từ `Point2d`:

```js
class Point2d { /* .. */ }

class Point3d extends Point2d {
    z
    constructor(x,y,z) {
        super(x,y);
        this.z = z;

        console.log(this.#ID);      // sẽ ném lỗi!
    }
}
```

| CẢNH BÁO: |
| :--- |
| Lưu ý rằng đoạn mã này ném ra lỗi cú pháp tĩnh sớm tại thời điểm định nghĩa lớp `Point3d`, trước khi có cơ hội tạo một thể hiện của lớp. Ngoại lệ tương tự sẽ được ném ra nếu tham chiếu là `super.#ID` thay vì `this.#ID`. |

#### Kiểm tra Sự Tồn tại

Hãy nhớ rằng chỉ bản thân `class` mới biết về, và do đó có thể kiểm tra, một trường/phương thức riêng tư như vậy.

Bạn có thể muốn kiểm tra xem một trường/phương thức riêng tư có tồn tại trên một thể hiện đối tượng hay không. Ví dụ (như được hiển thị bên dưới), bạn có thể có một hàm hoặc phương thức tĩnh trong một lớp, nhận một tham chiếu đối tượng bên ngoài được truyền vào. Để kiểm tra xem tham chiếu đối tượng được truyền vào có phải là cùng lớp này (và do đó có cùng các thành viên/phương thức riêng tư trong đó) hay không, về cơ bản bạn cần thực hiện "kiểm tra thương hiệu" (brand check) đối với đối tượng.

Một kiểm tra như vậy có thể khá phức tạp, bởi vì nếu bạn truy cập một trường riêng tư chưa tồn tại trên đối tượng, bạn sẽ nhận được một ngoại lệ JS được ném ra, yêu cầu logic `try..catch` xấu xí.

Nhưng có một cách tiếp cận sạch hơn, được gọi là "kiểm tra thương hiệu tiện dụng" (ergonomic brand check), sử dụng từ khóa `in`:

```js
class Point2d {
    // statics
    static samePoint(point1,point2) {
        // "kiểm tra thương hiệu tiện dụng"
        if (#ID in point1 && #ID in point2) {
            return point1.#ID === point2.#ID;
        }
        return false;
    }

    // privates
    #ID = null
    #assignID() {
        this.#ID = Math.round(Math.random() * 1e9);
    }

    // publics
    x
    y
    constructor(x,y) {
        this.#assignID();
        this.x = x;
        this.y = y;
    }
}

var one = new Point2d(3,4);
var two = new Point2d(3,4);

Point2d.samePoint(one,two);         // false
Point2d.samePoint(one,one);         // true
```

Kiểm tra `#privateField in someObject` sẽ không ném ra ngoại lệ nếu trường không được tìm thấy, vì vậy nó an toàn để sử dụng mà không cần `try..catch` và sử dụng kết quả boolean đơn giản của nó.

#### Rò rỉ (Exfiltration)

Mặc dù một thành viên/phương thức có thể được khai báo với khả năng hiển thị *private*, nó vẫn có thể bị rò rỉ (trích xuất) từ một thể hiện lớp:

```js
var id, func;

class Point2d {
    // privates
    #ID = null
    #assignID() {
        this.#ID = Math.round(Math.random() * 1e9);
    }

    // publics
    x
    y
    constructor(x,y) {
        this.#assignID();
        this.x = x;
        this.y = y;

        // rò rỉ
        id = this.#ID;
        func = this.#assignID;
    }
}

var point = new Point2d(3,4);

id;                     // 7392851012 (...ví dụ)

func;                   // function #assignID() { .. }
func.call(point,42);

func.call({},100);
// TypeError: Cannot write private member #ID to an
// object whose class did not declare it
```

Mối quan tâm chính ở đây là phải cẩn thận khi truyền các phương thức riêng tư làm callback (hoặc theo bất kỳ cách nào để lộ các private cho các phần khác của chương trình). Không có gì ngăn cản bạn làm như vậy, điều này có thể tạo ra một chút tiết lộ quyền riêng tư ngoài ý muốn.

### Tĩnh Riêng tư (Private Statics)

Các thuộc tính và hàm tĩnh cũng có thể sử dụng `#` để được đánh dấu là riêng tư:

```js
class Point2d {
    static #errorMsg = "Out of bounds."
    static #printError() {
        console.log(`Error: ${this.#errorMsg}`);
    }

    // publics
    x
    y
    constructor(x,y) {
        if (x > 100 || y > 100) {
            Point2d.#printError();
        }
        this.x = x;
        this.y = y;
    }
}

var one = new Point2d(30,400);
// Error: Out of bounds.
```

Hàm riêng tư tĩnh `#printError()` ở đây có một `this`, nhưng đó là tham chiếu đến lớp `Point2d`, không phải một thể hiện. Như vậy, `#errorMsg` và `#printError()` độc lập với các thể hiện và do đó tốt nhất là các static. Hơn nữa, không có lý do gì để chúng có thể truy cập được bên ngoài lớp, vì vậy chúng được đánh dấu là riêng tư.

Hãy nhớ: các static riêng tư cũng tương tự không được kế thừa bởi các lớp con giống như các thành viên/phương thức riêng tư không được kế thừa.

#### Cạm bẫy: Phân lớp Với Tĩnh Riêng tư và `this`

Hãy nhớ lại rằng các phương thức được kế thừa, được gọi từ một lớp con, không gặp khó khăn khi truy cập (thông qua các tham chiếu kiểu `this.#whatever`) bất kỳ private nào từ lớp cơ sở của chính chúng:

```js
class Point2d {
    // ..

    getID() {
        return this.#ID;
    }

    // ..
}

class Point3d extends Point2d {
    // ..

    printID() {
        console.log(`ID: ${this.getID()}`);
    }
}

var point = new Point3d(3,4,5);
point.printID();
// ID: ..
```

Điều đó hoạt động tốt.

Thật không may, và (đối với tôi) khá bất ngờ/không nhất quán, điều tương tự không đúng với các static riêng tư được truy cập từ các hàm tĩnh công khai được kế thừa:

```js
class Point2d {
    static #errorMsg = "Out of bounds."
    static printError() {
        console.log(`Error: ${this.#errorMsg}`);
    }

    // ..
}

class Point3d extends Point2d {
    // ..
}

Point2d.printError();
// Error: Out of bounds.

Point3d.printError === Point2d.printError;
// true

Point3d.printError();
// TypeError: Cannot read private member #errorMsg
// from an object whose class did not declare it
```

Hàm tĩnh `printError()` được kế thừa (chia sẻ qua `[[Prototype]]`) từ `Point2d` sang `Point3d` rất tốt, đó là lý do tại sao các tham chiếu hàm giống hệt nhau. Giống như đoạn mã không tĩnh ngay phía trên, bạn có thể đã mong đợi lệnh gọi tĩnh `Point3d.printError()` giải quyết thông qua chuỗi `[[Prototype]]` đến vị trí lớp cơ sở ban đầu của nó (`Point2d`), do đó cho phép nó truy cập static riêng tư `#errorMsg` của lớp cơ sở.

Nhưng nó thất bại, như được hiển thị bởi câu lệnh cuối cùng trong đoạn mã đó. Lý do nó thất bại ở đây, nhưng không phải với đoạn mã trước đó, là một câu đố xoắn não phức tạp. Tôi sẽ không đào sâu vào giải thích *tại sao* ở đây, thẳng thắn mà nói vì nó làm tôi sôi máu khi làm như vậy.

Tuy nhiên, có một *cách sửa*. Trong hàm tĩnh, thay vì `this.#errorMsg`, hãy đổi cái đó thành `Point2d.#errorMsg`, và bây giờ nó hoạt động:

```js
class Point2d {
    static #errorMsg = "Out of bounds."
    static printError() {
        // tham chiếu đã sửa vvvvvv
        console.log(`Error: ${Point2d.#errorMsg}`);
    }

    // ..
}

class Point3d extends Point2d {
    // ..
}

Point2d.printError();
// Error: Out of bounds.

Point3d.printError();
// Error: Out of bounds.  <-- phù, nó hoạt động rồi!
```

Nếu các hàm tĩnh công khai đang được kế thừa, hãy sử dụng tên lớp để truy cập bất kỳ static riêng tư nào thay vì sử dụng các tham chiếu `this.`. Hãy coi chừng cạm bẫy đó!

## Ví dụ Lớp

OK, chúng ta đã trình bày một loạt các tính năng lớp khác nhau. Tôi muốn kết thúc chương này bằng cách cố gắng minh họa một mẫu của các khả năng này trong một ví dụ duy nhất ít cơ bản/gượng ép hơn một chút.

```js
class CalendarItem {
    static #UNSET = Symbol("unset")
    static #isUnset(v) {
        return v === this.#UNSET;
    }
    static #error(num) {
        return this[`ERROR_${num}`];
    }
    static {
        for (let [idx,msg] of [
            "ID is already set.",
            "ID is unset.",
            "Don't instantiate 'CalendarItem' directly.",
        ].entries()) {
            this[`ERROR_${(idx+1)*100}`] = msg;
        }
    }
    static isSameItem(item1,item2) {
        if (#ID in item1 && #ID in item2) {
            return item1.#ID === item2.#ID;
        }
        else {
            return false;
        }
    }

    #ID = CalendarItem.#UNSET
    #setID(id) {
        if (CalendarItem.#isUnset(this.#ID)) {
            this.#ID = id;
        }
        else {
            throw new Error(CalendarItem.#error(100));
        }
    }

    description = null
    startDateTime = null

    constructor() {
        if (new.target !== CalendarItem) {
            let id = Math.round(Math.random() * 1e9);
            this.#setID(id);
        }
        else {
            throw new Error(CalendarItem.#error(300));
        }
    }
    getID() {
        if (!CalendarItem.#isUnset(this.#ID)) {
            return this.#ID;
        }
        else {
            throw new Error(CalendarItem.#error(200));
        }
    }
    getDateTimeStr() {
        if (this.startDateTime instanceof Date) {
            return this.startDateTime.toUTCString();
        }
    }
    summary() {
        console.log(`(${
            this.getID()
        }) ${
            this.description
        } at ${
            this.getDateTimeStr()
        }`);
    }
}

class Reminder extends CalendarItem {
    #complete = false;  // <-- không có ASI, cần dấu chấm phẩy

    [Symbol.toStringTag] = "Reminder"
    constructor(description,startDateTime) {
        super();

        this.description = description;
        this.startDateTime = startDateTime;
    }
    isComplete() {
        return !!this.#complete;
    }
    markComplete() {
        this.#complete = true;
    }
    summary() {
        if (this.isComplete()) {
            console.log(`(${this.getID()}) Complete.`);
        }
        else {
            super.summary();
        }
    }
}

class Meeting extends CalendarItem {
    #getEndDateTimeStr() {
        if (this.endDateTime instanceof Date) {
            return this.endDateTime.toUTCString();
        }
    }

    endDateTime = null;  // <-- không có ASI, cần dấu chấm phẩy

    [Symbol.toStringTag] = "Meeting"
    constructor(description,startDateTime,endDateTime) {
        super();

        this.description = description;
        this.startDateTime = startDateTime;
        this.endDateTime = endDateTime;
    }
    getDateTimeStr() {
        return `${
            super.getDateTimeStr()
        } - ${
            this.#getEndDateTimeStr()
        }`;
    }
}
```

Hãy dành chút thời gian để đọc và tiêu hóa các định nghĩa `class` đó. Bạn có phát hiện ra hầu hết các tính năng `class` chúng ta đã nói đến trong chương này không?

| LƯU Ý: |
| :--- |
| Một câu hỏi bạn có thể có: tại sao tôi không di chuyển logic lặp lại của việc thiết lập `description` và `startDateTime` từ cả hai constructor lớp con vào constructor cơ sở duy nhất? Đây là một điểm sắc thái, nhưng ý định của tôi không phải là `CalendarItem` bao giờ được khởi tạo trực tiếp; đó là cái mà trong thuật ngữ hướng lớp chúng ta gọi là một "lớp trừu tượng" (abstract class). Đó là lý do tại sao tôi đang sử dụng `new.target` để ném lỗi nếu lớp `CalendarItem` bao giờ được khởi tạo trực tiếp! Vì vậy, tôi không muốn ngụ ý bằng chữ ký rằng constructor `CalendarItem(..)` nên bao giờ được sử dụng trực tiếp. |

Bây giờ hãy xem ba lớp này được sử dụng:

```js
var callMyParents = new Reminder(
    "Call my parents to say hi",
    new Date("July 7, 2022 11:00:00 UTC")
);
callMyParents.toString();
// [object Reminder]
callMyParents.summary();
// (586380912) Call my parents to say hi at
// Thu, 07 Jul 2022 11:00:00 GMT
callMyParents.markComplete();
callMyParents.summary();
// (586380912) Complete.
callMyParents instanceof Reminder;
// true
callMyParents instanceof CalendarItem;
// true
callMyParents instanceof Meeting;
// false


var interview = new Meeting(
    "Job Interview: ABC Tech",
    new Date("June 23, 2022 08:30:00 UTC"),
    new Date("June 23, 2022 09:15:00 UTC")
);
interview.toString();
// [object Meeting]
interview.summary();
// (994337604) Job Interview: ABC Tech at Thu,
// 23 Jun 2022 08:30:00 GMT - Thu, 23 Jun 2022
// 09:15:00 GMT
interview instanceof Meeting;
// true
interview instanceof CalendarItem;
// true
interview instanceof Reminder;
// false


Reminder.isSameItem(callMyParents,callMyParents);
// true
Meeting.isSameItem(callMyParents,interview);
// false
```

Phải thừa nhận rằng, một số phần của ví dụ này hơi gượng ép. Nhưng thành thật mà nói, tôi nghĩ khá nhiều trong số này là cách sử dụng hợp lý và hợp lý của các tính năng `class` khác nhau.

Nhân tiện, có lẽ có một triệu cách khác nhau để cấu trúc logic mã ở trên. Tôi không có ý định tuyên bố đây là cách *đúng* hoặc *tốt nhất* để làm như vậy. Như một bài tập cho người đọc, hãy thử sức và tự viết nó, và ghi chú những điều bạn đã làm khác với cách tiếp cận của tôi.

[^POLP]: "Principle of Least Privilege", Wikipedia; https://en.wikipedia.org/wiki/Principle_of_least_privilege ; Truy cập tháng 7 năm 2022
