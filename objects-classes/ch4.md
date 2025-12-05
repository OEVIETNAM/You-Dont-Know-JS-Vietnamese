# You Don't Know JS Yet: Đối tượng & Lớp - Ấn bản thứ 2
# Chương 4: `this` Hoạt Động

| LƯU Ý: |
| :--- |
| Đang trong quá trình thực hiện |

Chúng ta đã thấy từ khóa `this` được sử dụng khá nhiều cho đến nay, nhưng chưa thực sự đào sâu để hiểu chính xác cách nó hoạt động trong JS. Đã đến lúc chúng ta làm điều đó.

Nhưng để hiểu đúng về `this` trong JS, bạn cần gạt sang một bên bất kỳ định kiến nào bạn có thể có, đặc biệt là các giả định từ cách `this` hoạt động trong các ngôn ngữ lập trình khác mà bạn có thể có kinh nghiệm.

Đây là điều quan trọng nhất cần hiểu về `this`: việc xác định giá trị nào (thường là đối tượng) mà `this` trỏ vào không được thực hiện tại thời điểm viết mã (author time), mà được xác định tại thời điểm chạy (runtime). Điều đó có nghĩa là bạn không thể chỉ nhìn vào một hàm nhận biết `this` (ngay cả một phương thức trong định nghĩa `class`) và biết chắc chắn `this` sẽ giữ gì trong khi hàm đó chạy.

Thay vào đó, bạn phải tìm từng nơi hàm được gọi, và xem xét *cách* nó được gọi (thậm chí *ở đâu* cũng không quan trọng). Đó là cách duy nhất để trả lời đầy đủ `this` sẽ trỏ đến cái gì.

Trên thực tế, một hàm nhận biết `this` duy nhất có thể được gọi theo ít nhất bốn cách khác nhau, và bất kỳ cách tiếp cận nào trong số đó cũng sẽ dẫn đến việc gán một `this` khác nhau cho lệnh gọi hàm cụ thể đó.

Vì vậy, câu hỏi điển hình mà chúng ta có thể hỏi khi đọc mã -- "`this` trỏ đến cái gì trong hàm?" -- thực sự không phải là một câu hỏi hợp lệ. Câu hỏi bạn thực sự phải hỏi là, "Khi hàm được gọi theo một cách nhất định, `this` nào sẽ được gán cho lệnh gọi đó?"

Nếu não bạn đã xoắn lại chỉ khi đọc phần giới thiệu chương này... tốt! Hãy chuẩn bị cho việc nối lại dây thần kinh về cách bạn nghĩ về `this` trong JS.

## Nhận biết `this` (This Aware)

Tôi đã sử dụng cụm từ nhận biết `this` (this-aware) một lúc trước. Nhưng chính xác ý tôi là gì?

Bất kỳ hàm nào có từ khóa `this` trong đó.

Nếu một hàm không có `this` trong đó ở bất cứ đâu, thì các quy tắc về cách `this` hành xử không ảnh hưởng đến hàm đó theo bất kỳ cách nào. Nhưng nếu nó *có* dù chỉ một `this` trong đó, thì bạn hoàn toàn không thể xác định hàm sẽ hành xử như thế nào mà không tìm ra, cho mỗi lần gọi hàm, `this` sẽ trỏ đến cái gì.

Nó giống như từ khóa `this` là một trình giữ chỗ (placeholder) trong một mẫu. Việc thay thế giá trị của trình giữ chỗ đó không được xác định khi chúng ta viết mã; nó được xác định trong khi mã đang chạy.

Bạn có thể nghĩ rằng tôi chỉ đang chơi trò chơi chữ ở đây. Tất nhiên, khi bạn viết chương trình, bạn viết ra tất cả các lệnh gọi đến từng hàm, vì vậy bạn đã xác định `this` sẽ là gì khi bạn viết mã, phải không? Phải không!?

Không nhanh thế đâu!

Trước hết, bạn không phải lúc nào cũng viết tất cả mã gọi (các) hàm của mình. (Các) hàm nhận biết `this` của bạn có thể được truyền dưới dạng callback cho một số mã khác, hoặc trong cơ sở mã của bạn, hoặc trong khung/tiện ích của bên thứ ba, hoặc thậm chí bên trong cơ chế tích hợp sẵn của ngôn ngữ hoặc môi trường đang lưu trữ chương trình của bạn.

Nhưng ngay cả khi không truyền các hàm dưới dạng callback, một số cơ chế trong JS cho phép các hành vi thời gian chạy có điều kiện xác định giá trị nào (một lần nữa, thường là đối tượng) sẽ được đặt cho `this` của một lệnh gọi hàm cụ thể. Vì vậy, mặc dù bạn có thể đã viết tất cả mã đó, bạn *tốt nhất* sẽ phải thực thi trong đầu các điều kiện/đường dẫn khác nhau dẫn đến việc ảnh hưởng đến lệnh gọi hàm.

Và tại sao tất cả điều này lại quan trọng?

Bởi vì không chỉ bạn, tác giả của mã, cần phải tìm ra những thứ này. Đó là *mọi độc giả* của mã của bạn, mãi mãi. Nếu bất kỳ ai (thậm chí là bản thân bạn trong tương lai) muốn đọc một đoạn mã định nghĩa một hàm nhận biết `this`, điều đó chắc chắn có nghĩa là, để hiểu đầy đủ và dự đoán hành vi của nó, người đó sẽ phải tìm, đọc và hiểu từng lệnh gọi của hàm đó.

### `this` Làm Tôi Bối Rối (This Confuses Me)

Bây giờ, công bằng mà nói, điều đó đã đúng một phần nếu chúng ta xem xét các tham số của một hàm. Để hiểu một hàm sẽ hoạt động như thế nào, chúng ta cần biết những gì đang được truyền vào nó. Vì vậy, bất kỳ hàm nào có ít nhất một tham số, theo một nghĩa tương tự, là nhận biết *đối số* -- nghĩa là, (các) đối số nào được truyền vào và gán cho (các) tham số của hàm.

Nhưng với các tham số, chúng ta thường có thêm một chút gợi ý từ chính hàm về những gì các tham số sẽ làm và giữ.

Chúng ta thường thấy tên của các tham số được khai báo ngay trong tiêu đề hàm, điều này giúp ích rất nhiều trong việc giải thích bản chất/mục đích của chúng. Và nếu có các giá trị mặc định cho các tham số, chúng ta thường thấy chúng được khai báo nội tuyến với các mệnh đề `= whatever`. Hơn nữa, tùy thuộc vào phong cách mã của tác giả, chúng ta có thể thấy trong vài dòng đầu tiên của hàm một tập hợp logic áp dụng cho các tham số này; đây có thể là các khẳng định về các giá trị (các giá trị không được phép, v.v.), hoặc thậm chí các sửa đổi (chuyển đổi kiểu, định dạng, v.v.).

Thực ra, `this` rất giống một tham số cho một hàm, nhưng nó là một tham số ngầm định thay vì một tham số rõ ràng. Bạn không thấy bất kỳ tín hiệu nào cho thấy `this` sẽ được sử dụng, trong tiêu đề hàm ở bất cứ đâu. Bạn phải đọc toàn bộ thân hàm để xem liệu `this` có xuất hiện ở đâu không.

Tên "tham số" luôn là `this`, vì vậy chúng ta không nhận được nhiều gợi ý về bản chất/mục đích của nó từ một cái tên chung chung như vậy. Trên thực tế, về mặt lịch sử, có rất nhiều sự nhầm lẫn về việc "this" thậm chí được cho là có nghĩa gì. Và chúng ta hiếm khi thấy nhiều nếu có bất cứ điều gì được thực hiện để xác thực/chuyển đổi/v.v. giá trị `this` được áp dụng cho một lệnh gọi hàm. Trên thực tế, hầu như tất cả mã nhận biết `this` mà tôi từng thấy chỉ giả định gọn gàng rằng "tham số" `this` đang giữ chính xác giá trị được mong đợi. Nói về **một cái bẫy cho các lỗi không mong muốn!**

### Vậy `this` Là Gì? (So What Is This?)

Nếu `this` là một tham số ngầm định, mục đích của nó là gì? Cái gì đang được truyền vào?

Hy vọng rằng bạn đã đọc cuốn sách "Phạm vi & Closures" của bộ này. Nếu chưa, tôi thực sự khuyến khích bạn quay lại và đọc cuốn đó sau khi bạn hoàn thành cuốn này. Trong cuốn sách đó, tôi đã giải thích rất dài về cách phạm vi (và closures!) hoạt động, một đặc điểm đặc biệt quan trọng của các hàm.

Phạm vi từ vựng (bao gồm tất cả các biến được đóng lại) đại diện cho một ngữ cảnh *tĩnh* để các tham chiếu định danh từ vựng của hàm được đánh giá dựa trên đó. Nó cố định/tĩnh bởi vì tại thời điểm viết mã, khi bạn đặt các hàm và khai báo biến trong các phạm vi (lồng nhau) khác nhau, các quyết định đó là cố định, và không bị ảnh hưởng bởi bất kỳ điều kiện thời gian chạy nào.

Ngược lại, một ngôn ngữ lập trình khác có thể cung cấp phạm vi *động*, trong đó ngữ cảnh cho các tham chiếu biến của một hàm không được xác định bởi các quyết định tại thời điểm viết mã mà bởi các điều kiện thời gian chạy. Một hệ thống như vậy chắc chắn sẽ linh hoạt hơn ngữ cảnh tĩnh -- mặc dù sự linh hoạt thường đi kèm với sự phức tạp.

Để rõ ràng: phạm vi JS luôn luôn và chỉ là từ vựng và *tĩnh* (nếu chúng ta bỏ qua các trò gian lận chế độ không nghiêm ngặt như `eval(..)` và `with`). Tuy nhiên, một trong những điều thực sự mạnh mẽ về JS là nó cung cấp một cơ chế khác với sự linh hoạt và khả năng tương tự như phạm vi *động*.

Cơ chế `this`, thực sự, là ngữ cảnh *động* (không phải phạm vi); đó là cách một hàm nhận biết `this` có thể được gọi động dựa trên các ngữ cảnh khác nhau -- điều không thể thực hiện được với closure và các định danh phạm vi từ vựng!

### Tại Sao `this` Lại Ngầm Định Như Vậy?

Bạn có thể tự hỏi tại sao một cái gì đó quan trọng như một ngữ cảnh *động* lại được xử lý như một đầu vào ngầm định cho một hàm, thay vì là một đối số rõ ràng được truyền vào.

Đó là một câu hỏi rất quan trọng, nhưng nó không phải là câu hỏi chúng ta có thể trả lời ngay bây giờ. Tuy nhiên, hãy giữ câu hỏi đó.

### Chúng Ta Có Thể Tiếp Tục Với Điều Này Không? (Can We Get On With This?)

Vậy tại sao tôi lại nói dông dài về chủ đề *này* trong vài trang rồi? Bạn hiểu rồi, phải không!? Bạn đã sẵn sàng để tiếp tục.

Quan điểm của tôi là, bạn, tác giả của mã, và tất cả những người đọc mã khác thậm chí nhiều năm hoặc nhiều thập kỷ trong tương lai, cần phải nhận biết `this`. Đó là sự lựa chọn, gánh nặng, mà bạn đặt lên việc đọc mã như vậy. Và vâng, điều đó cũng áp dụng cho sự lựa chọn sử dụng `class` (xem Chương 3), vì hầu hết các phương thức lớp sẽ nhận biết `this` do sự cần thiết.

Hãy nhận biết về sự lựa chọn `this` *này* trong mã bạn viết. Hãy làm điều đó một cách có chủ ý, và làm điều đó theo cách tạo ra nhiều lợi ích kết quả hơn là gánh nặng. Hãy chắc chắn rằng việc sử dụng `this` trong mã của bạn *xứng đáng với trọng lượng của nó*.

Hãy để tôi nói theo cách *này*: đừng sử dụng mã nhận biết `this` trừ khi bạn thực sự có thể biện minh cho nó, và bạn đã cân nhắc kỹ lưỡng các chi phí. Chỉ vì bạn đã thấy rất nhiều ví dụ mã sử dụng `this` trong mã của người khác, không có nghĩa là `this` thuộc về mã *này* mà bạn đang viết.

Cơ chế `this` trong JS, kết hợp với ủy quyền `[[Prototype]]`, là một trụ cột cực kỳ mạnh mẽ của ngôn ngữ. Nhưng như câu nói sáo rỗng: "sức mạnh lớn đi kèm với trách nhiệm lớn". Theo giai thoại, mặc dù tôi thực sự thích và đánh giá cao trụ cột *này* của JS, có lẽ ít hơn 5% mã JS tôi từng viết sử dụng nó. Và khi tôi làm vậy, đó là với sự kiềm chế. Nó không phải là khả năng JS mặc định, hay dùng của tôi.

## Chính Là Nó! (This Is It!)

OK, đủ bài giảng dài dòng rồi. Bạn đã sẵn sàng để đi sâu vào mã `this`, phải không?

Hãy xem lại (và mở rộng) `Point2d` từ Chương 3, nhưng chỉ như một đối tượng với các thuộc tính dữ liệu và hàm trên đó, thay vì sử dụng `class`:

```js
var point = {
    x: null,
    y: null,

    init(x,y) {
        this.x = x;
        this.y = y;
    },
    rotate(angleRadians) {
        var rotatedX = this.x * Math.cos(angleRadians) -
            this.y * Math.sin(angleRadians);
        var rotatedY = this.x * Math.sin(angleRadians) +
            this.y * Math.cos(angleRadians);
        this.x = rotatedX;
        this.y = rotatedY;
    },
    toString() {
        return `(${this.x},${this.y})`;
    },
};
```

Như bạn có thể thấy, các hàm `init(..)`, `rotate(..)`, và `toString()` là nhận biết `this`. Bạn có thể có thói quen giả định rằng tham chiếu `this` rõ ràng sẽ luôn giữ đối tượng `point`. Nhưng điều đó không được đảm bảo theo bất kỳ cách nào.

Hãy tiếp tục nhắc nhở bản thân khi bạn đi qua phần còn lại của chương này: giá trị `this` cho một hàm được xác định bởi *cách* hàm được gọi. Điều đó có nghĩa là bạn không thể nhìn vào định nghĩa của hàm, cũng như nơi hàm được định nghĩa (thậm chí không phải `class` bao quanh!). Trên thực tế, thậm chí không quan trọng hàm được gọi từ đâu.

Chúng ta chỉ cần nhìn vào *cách* các hàm được gọi; đó là yếu tố duy nhất quan trọng.

### Gọi Ngữ cảnh Ngầm định (Implicit Context Invocation)

Hãy xem xét cuộc gọi này:

```js
point.init(3,4);
```

Chúng ta đang gọi hàm `init(..)`, nhưng hãy chú ý đến `point.` ở phía trước nó? Đây là một ràng buộc *ngữ cảnh ngầm định*. Nó nói với JS: gọi hàm `init(..)` với `this` tham chiếu đến `point`.

Đó là cách *bình thường* chúng ta mong đợi một `this` hoạt động, và đó cũng là một trong những cách phổ biến nhất chúng ta gọi các hàm. Vì vậy, lệnh gọi điển hình mang lại cho chúng ta kết quả trực quan. Đó là một điều tốt!

### Gọi Ngữ cảnh Mặc định (Default Context Invocation)

Nhưng điều gì xảy ra nếu chúng ta làm điều này?

```js
const init = point.init;
init(3,4);
```

Bạn có thể cho rằng chúng ta sẽ nhận được kết quả tương tự như đoạn mã trước. Nhưng đó không phải là cách gán `this` của JS hoạt động.

*Vị trí gọi* (call-site) cho hàm là `init(3,4)`, khác với `point.init(3,4)`. Khi không có *ngữ cảnh ngầm định* (`point.`), cũng như bất kỳ cơ chế gán `this` nào khác, việc gán *ngữ cảnh mặc định* sẽ xảy ra.

`this` sẽ tham chiếu đến cái gì khi `init(3,4)` được gọi như vậy?

*Nó phụ thuộc.*

Ồ ồ. Phụ thuộc? Nghe có vẻ khó hiểu.

Đừng lo lắng, nó không tệ như bạn nghĩ đâu. Việc gán *ngữ cảnh mặc định* phụ thuộc vào việc mã có ở chế độ nghiêm ngặt (strict-mode) hay không. Nhưng rất may, hầu như tất cả mã JS ngày nay đều đang chạy ở chế độ nghiêm ngặt; ví dụ, ESM (ES Modules) luôn chạy ở chế độ nghiêm ngặt, cũng như mã bên trong một khối `class`. Và hầu như tất cả mã JS được chuyển mã (transpiled) (thông qua Babel, TypeScript, v.v.) đều được viết để khai báo chế độ nghiêm ngặt.

Vì vậy, hầu như mọi lúc, mã JS hiện đại sẽ chạy ở chế độ nghiêm ngặt, và do đó ngữ cảnh *gán mặc định* sẽ không "phụ thuộc" vào bất cứ điều gì; nó khá đơn giản: `undefined`. Thế thôi!

| LƯU Ý: |
| :--- |
| Hãy nhớ rằng: `undefined` không có nghĩa là "không được định nghĩa"; nó có nghĩa là, "được định nghĩa với giá trị `undefined` rỗng đặc biệt". Tôi biết, tôi biết... tên và ý nghĩa không khớp nhau. Đó là hành lý di sản ngôn ngữ, dành cho bạn. (nhún vai) |

Điều đó có nghĩa là `init(3,4)`, nếu chạy ở chế độ nghiêm ngặt, sẽ ném ra một ngoại lệ. Tại sao? Bởi vì tham chiếu `this.x` trong `init(..)` là một truy cập thuộc tính `.x` trên `undefined` (tức là `undefined.x`), điều này không được phép:

```js
"use strict";

var point = { /* .. */ };

const init = point.init;
init(3,4);
// TypeError: Cannot set properties of
// undefined (setting 'x')
```

Hãy dừng lại một chút và xem xét: tại sao JS lại chọn mặc định ngữ cảnh thành `undefined`, để bất kỳ lệnh gọi *ngữ cảnh mặc định* nào của một hàm nhận biết `this` sẽ thất bại với một ngoại lệ như vậy?

Bởi vì một hàm nhận biết `this` **luôn cần một `this`**. Lệnh gọi `init(3,4)` không cung cấp một `this`, vì vậy đó *là* một sai lầm, và *nên* đưa ra một ngoại lệ để sai lầm có thể được sửa chữa. Bài học: không bao giờ gọi một hàm nhận biết `this` mà không cung cấp cho nó một `this`!

Chỉ để cho đầy đủ: trong chế độ không nghiêm ngặt ít phổ biến hơn, *ngữ cảnh mặc định* là đối tượng toàn cục -- JS định nghĩa nó là `globalThis`, trong trình duyệt JS về cơ bản là bí danh của `window`, và trong Node nó là `global`. Vì vậy, khi `init(3,4)` chạy trong chế độ không nghiêm ngặt, biểu thức `this.x` là `globalThis.x` -- còn được gọi là `window.x` trong trình duyệt, hoặc `global.x` trong Node. Do đó, `globalThis.x` được đặt là `3` và `globalThis.y` được đặt là `4`.

```js
// không có chế độ nghiêm ngặt ở đây, hãy coi chừng!

var point = { /* .. */ };

const init = point.init;
init(3,4);

globalThis.x;   // 3
globalThis.y;   // 4
point.x;        // null
point.y;        // null
```

Điều đó thật đáng tiếc, bởi vì nó gần như chắc chắn *không phải* là kết quả mong muốn. Nó không chỉ tệ nếu nó là một biến toàn cục, mà nó còn *không* thay đổi thuộc tính trên đối tượng `point` của chúng ta, vì vậy lỗi chương trình được đảm bảo.

| CẢNH BÁO: |
| :--- |
| Ái chà! Không ai muốn các biến toàn cục ngẫu nhiên được tạo ngầm từ khắp nơi trong mã. Bài học: luôn đảm bảo mã của bạn đang chạy ở chế độ nghiêm ngặt! |

### Gọi Ngữ cảnh Rõ ràng (Explicit Context Invocation)

Các hàm có thể được gọi thay thế với *ngữ cảnh rõ ràng*, sử dụng các tiện ích tích hợp `call(..)` hoặc `apply(..)`:

```js
var point = { /* .. */ };

const init = point.init;

init.call( point, 3, 4 );
// hoặc: init.apply( point, [ 3, 4 ] )

point.x;        // 3
point.y;        // 4
```

`init.call(point,3,4)` thực sự giống như `point.init(3,4)`, ở chỗ cả hai đều gán `point` làm ngữ cảnh `this` cho lệnh gọi `init(..)`.

| LƯU Ý: |
| :--- |
| Cả hai tiện ích `call(..)` và `apply(..)` đều lấy đối số đầu tiên của chúng là một giá trị ngữ cảnh `this`; đó hầu như luôn là một đối tượng, nhưng về mặt kỹ thuật có thể là bất kỳ giá trị nào (số, chuỗi, v.v.). Tiện ích `call(..)` lấy các đối số tiếp theo và truyền chúng qua hàm được gọi, trong khi `apply(..)` mong đợi đối số thứ hai của nó là một mảng các giá trị để truyền làm đối số. |

Có vẻ khó xử khi xem xét việc gọi một hàm với kiểu gán *ngữ cảnh rõ ràng* (`call(..)` / `apply(..)`) trong chương trình của bạn. Nhưng nó hữu ích hơn những gì có thể thấy rõ ràng ngay từ cái nhìn đầu tiên.

Hãy nhớ lại đoạn mã gốc:

```js
var point = {
    x: null,
    y: null,

    init(x,y) {
        this.x = x;
        this.y = y;
    },
    rotate(angleRadians) { /* .. */ },
    toString() {
        return `(${this.x},${this.y})`;
    },
};

point.init(3,4);

var anotherPoint = {};
point.init.call( anotherPoint, 5, 6 );

point.x;                // 3
point.y;                // 4
anotherPoint.x;         // 5
anotherPoint.y;         // 6
```

Bạn có thấy những gì tôi đã làm ở đó không?

Tôi muốn định nghĩa `anotherPoint`, nhưng tôi không muốn lặp lại các định nghĩa của các hàm `init(..)` / `rotate(..)` / `toString()` đó từ `point`. Vì vậy, tôi đã "mượn" một tham chiếu hàm, `point.init`, và đặt rõ ràng đối tượng rỗng `anotherPoint` làm ngữ cảnh `this`, thông qua `call(..)`.

Khi `init(..)` đang chạy tại thời điểm đó, `this` bên trong nó sẽ tham chiếu đến `anotherPoint`, và đó là lý do tại sao các thuộc tính `x` / `y` (giá trị `5` / `6`, tương ứng) được đặt ở đó.

Bất kỳ hàm nhận biết `this` nào cũng có thể được mượn như thế này: `point.rotate.call(anotherPoint, ..)`, `point.toString.call(anotherPoint)`.

#### Xem Lại Gọi Ngữ cảnh Ngầm định

Một cách tiếp cận khác để chia sẻ hành vi giữa `point` và `anotherPoint` sẽ là:

```js
var point = { /* .. */ };

var anotherPoint = {
    init: point.init,
    rotate: point.rotate,
    toString: point.toString,
};

anotherPoint.init(5,6);

anotherPoint.x;         // 5
anotherPoint.y;         // 6
```

Đây là một cách khác để "mượn" các hàm, bằng cách thêm các tham chiếu được chia sẻ vào các hàm trên bất kỳ đối tượng đích nào (ví dụ: `anotherPoint`). Lệnh gọi tại vị trí gọi `anotherPoint.init(5,6)` là kiểu tự nhiên/tiện dụng hơn dựa trên việc gán *ngữ cảnh ngầm định*.

Có vẻ như cách tiếp cận này sạch hơn một chút, so sánh `anotherPoint.init(5,6)` với `point.init.call(anotherPoint,5,6)`.

Nhưng nhược điểm chính là phải sửa đổi bất kỳ đối tượng đích nào với các tham chiếu hàm được chia sẻ như vậy, điều này có thể dài dòng, thủ công và dễ bị lỗi. Đôi khi cách tiếp cận như vậy là chấp nhận được, nhưng nhiều lần khác, việc gán *ngữ cảnh rõ ràng* với `call(..)` / `apply(..)` được ưu tiên hơn.

### Gọi Ngữ cảnh Mới (New Context Invocation)

Cho đến nay, chúng ta đã thấy ba cách gán ngữ cảnh khác nhau tại vị trí gọi hàm: *mặc định*, *ngầm định*, và *rõ ràng*.

Cách thứ tư để gọi một hàm, và gán `this` cho lệnh gọi đó, là với từ khóa `new`:

```js
var point = {
    // ..

    init: function() { /* .. */ }

    // ..
};

var anotherPoint = new point.init(3,4);

anotherPoint.x;     // 3
anotherPoint.y;     // 4
```

| MẸO: |
| :--- |
| Ví dụ này có một chút sắc thái cần được giải thích. Dạng `init: function() { .. }` được hiển thị ở đây -- cụ thể là một biểu thức hàm được gán cho một thuộc tính -- là bắt buộc để hàm được gọi hợp lệ với từ khóa `new`. Từ các đoạn mã trước, dạng phương thức ngắn gọn của `init() { .. }` định nghĩa một hàm *không thể* được gọi với `new`. |

Bạn thường thấy `new` được sử dụng với `class` để tạo các thể hiện. Nhưng như một cơ chế cơ bản của ngôn ngữ JS, `new` vốn không phải là một hoạt động `class`.

Theo một nghĩa nào đó, từ khóa `new` chiếm quyền điều khiển một hàm và buộc hành vi của nó vào một chế độ khác so với một lệnh gọi bình thường. Dưới đây là 4 bước đặc biệt mà JS thực hiện khi một hàm được gọi với `new`:

1. tạo một đối tượng rỗng hoàn toàn mới, từ hư không.

2. liên kết `[[Prototype]]` của đối tượng rỗng mới đó với đối tượng `.prototype` của hàm (xem Chương 2).

3. gọi hàm với ngữ cảnh `this` được đặt thành đối tượng rỗng mới đó.

4. nếu hàm không trả về giá trị đối tượng của riêng nó một cách rõ ràng (với câu lệnh `return ..`), giả sử lệnh gọi hàm thay vào đó sẽ trả về đối tượng mới (từ các bước 1-3).

| CẢNH BÁO: |
| :--- |
| Bước 4 ngụ ý rằng nếu bạn gọi `new` một hàm mà *có* trả về đối tượng của riêng nó -- như `return { .. }`, v.v. -- thì đối tượng mới từ các bước 1-3 sẽ *không* được trả về. Đó là một cạm bẫy khó khăn cần lưu ý, ở chỗ nó loại bỏ hiệu quả đối tượng mới đó trước khi chương trình có cơ hội nhận và lưu trữ một tham chiếu đến nó. Về cơ bản, `new` không bao giờ nên được sử dụng để gọi một hàm có (các) câu lệnh `return ..` rõ ràng trong đó. |

Để hiểu 4 bước `new` này một cách cụ thể hơn, tôi sẽ minh họa chúng bằng mã, như một sự thay thế cho việc sử dụng từ khóa `new`:

```js
// thay thế cho:
//   var anotherPoint = new point.init(3,4)

var anotherPoint;
// đây là một khối trần để ẩn các khai báo
// `let` cục bộ
{
    // (Bước 1)
    let tmpObj = {};

    // (Bước 2)
    Object.setPrototypeOf(
        tmpObj, point.init.prototype
    );
    // hoặc: tmpObj.__proto__ = point.init.prototype

    // (Bước 3)
    let res = point.init.call(tmpObj,3,4);

    // (Bước 4)
    anotherPoint = (
        typeof res !== "object" ? tmpObj : res
    );
}
```

Rõ ràng, lệnh gọi `new` sắp xếp hợp lý tập hợp các bước thủ công đó!

| MẸO: |
| :--- |
| `Object.setPrototypeOf(..)` trong bước 2 cũng có thể được thực hiện thông qua thuộc tính `__proto__`, chẳng hạn như `tmpObj.__proto__ = point.init.prototype`, hoặc thậm chí là một phần của object literal (bước 1) với `tmpObj = { __proto__: point.init.prototype }`. |

Bỏ qua một số hình thức của các bước này, hãy nhớ lại một đoạn mã trước đó và xem cách `new` xấp xỉ một kết quả tương tự:

```js
var point = { /* .. */ };

// cách tiếp cận này:
var anotherPoint = {};
point.init.call(anotherPoint,5,6);

// thay vào đó có thể được xấp xỉ như:
var yetAnotherPoint = new point.init(5,6);
```

Điều đó tốt hơn một chút! Nhưng có một cảnh báo ở đây.

Sử dụng các hàm khác mà `point` giữ đối với `anotherPoint` / `yetAnotherPoint`, chúng ta sẽ không muốn làm với `new`. Tại sao? Bởi vì `new` đang tạo một đối tượng *mới*, nhưng đó không phải là những gì chúng ta muốn nếu chúng ta có ý định gọi một hàm đối với một đối tượng hiện có.

Thay vào đó, chúng ta có thể sẽ sử dụng gán *ngữ cảnh rõ ràng*:

```js
point.rotate.call( anotherPoint, /*angleRadians=*/Math.PI );

point.toString.call( yetAnotherPoint );
// (5,6)
```

### Xem Lại Điều Này (Review This)

Chúng ta đã thấy bốn quy tắc cho việc gán ngữ cảnh `this` trong các lệnh gọi hàm. Hãy sắp xếp chúng theo thứ tự ưu tiên:

1. Hàm có được gọi với `new`, tạo và thiết lập một `this` *mới* không?

2. Hàm có được gọi với `call(..)` hoặc `apply(..)`, thiết lập *rõ ràng* `this` không?

3. Hàm có được gọi với một tham chiếu đối tượng tại vị trí gọi (ví dụ: `point.init(..)`), thiết lập *ngầm định* `this` không?

4. Nếu không có điều nào ở trên... chúng ta có đang ở chế độ không nghiêm ngặt không? Nếu vậy, *mặc định* `this` thành `globalThis`. Nhưng nếu ở chế độ nghiêm ngặt, *mặc định* `this` thành `undefined`.

Các quy tắc này, *theo thứ tự này*, là cách JS xác định `this` cho một lệnh gọi hàm. Nếu nhiều quy tắc khớp với một vị trí gọi (ví dụ: `new point.init.call(..)`), quy tắc đầu tiên từ danh sách khớp sẽ thắng.

Thế đấy, bây giờ bạn đã làm chủ từ khóa `this`. Chà, không hẳn. Còn rất nhiều sắc thái cần đề cập. Nhưng bạn đang đi đúng hướng!

## Một Mũi Tên Trỏ Đến Đâu Đó (An Arrow Points Somewhere)

Mọi thứ tôi đã khẳng định cho đến nay về `this` trong các hàm, và cách nó được xác định dựa trên vị trí gọi, đều đưa ra một giả định khổng lồ: rằng bạn đang xử lý một hàm *thông thường* (hoặc phương thức).

Vậy một hàm *bất thường* là gì?!? Nó trông giống như thế này:

```js
const x = x => x <= x;
```

| LƯU Ý: |
| :--- |
| Vâng, tôi đang hơi mỉa mai và không công bằng khi gọi một hàm mũi tên là "bất thường" và sử dụng một ví dụ gượng ép như vậy. Đó là một trò đùa, được chứ? |

Đây là một ví dụ thực tế về hàm mũi tên `=>`:

```js
const clickHandler = evt =>
    evt.target.matches("button") ?
        this.theFormElem.submit() :
        evt.stopPropagation();
```

Để so sánh, hãy để tôi cũng hiển thị tương đương không phải mũi tên:

```js
const clickHandler = function(evt) {
    evt.target.matches("button") ?
        this.theFormElem.submit() :
        evt.stopPropagation();
};
```

Hoặc nếu chúng ta đi theo phong cách cũ một chút -- đây là sở thích của tôi! -- chúng ta có thể thử dạng khai báo hàm độc lập:

```js
function clickHandler(evt) {
    evt.target.matches("button") ?
        this.theFormElem.submit() :
        evt.stopPropagation();
}
```

Hoặc nếu hàm xuất hiện dưới dạng một phương thức trong định nghĩa `class`, hoặc dưới dạng một phương thức ngắn gọn trong object literal, nó sẽ trông giống như thế này:

```js
// ..
clickHandler(evt) {
    evt.target.matches("button") ?
        this.theFormElem.submit() :
        evt.stopPropagation();
}
```

Điều tôi thực sự muốn tập trung vào là cách mỗi dạng hàm này sẽ hành xử đối với tham chiếu `this` của chúng, và liệu dạng `=>` đầu tiên có khác với các dạng khác hay không (gợi ý: có!). Nhưng hãy bắt đầu với một bài kiểm tra nhỏ để xem bạn có chú ý không.

Đối với mỗi dạng hàm vừa được hiển thị, làm thế nào chúng ta biết mỗi `this` sẽ tham chiếu đến cái gì?

### Vị trí Gọi Ở Đâu? (Where's The Call-site?)

Hy vọng rằng, bạn đã trả lời một cái gì đó như: "đầu tiên, chúng ta cần xem các hàm được gọi như thế nào."

Đủ công bằng.

Giả sử chương trình của chúng ta trông giống như thế này:

```js
var infoForm = {
    theFormElem: null,
    theSubmitBtn: null,

    init() {
        this.theFormElem =
            document.getElementById("the-info-form");
        this.theSubmitBtn =
            theFormElem.querySelector("button[type=submit]");

        // đây có phải là vị trí gọi không?
        this.theSubmitBtn.addEventListener(
            "click",
            this.clickHandler,
            false
        );
    },

    // ..
}
```

À, thú vị. Một nửa số độc giả của các bạn chưa bao giờ thấy mã DOM API thực tế như `getElementById(..)`, `querySelector(..)`, và `addEventListener(..)` trước đây. Tôi nghe thấy tiếng chuông bối rối vang lên vừa rồi!

| LƯU Ý: |
| :--- |
| Xin lỗi, tôi đang tiết lộ tuổi tác của mình ở đây. Tôi đã làm những thứ này đủ lâu để nhớ khi chúng tôi làm loại mã đó rất lâu trước khi chúng tôi có các tiện ích như jQuery làm lộn xộn mã với `$` ở khắp mọi nơi. Và sau nhiều năm phát triển front-end, chúng ta dường như đã hạ cánh ở đâu đó "hiện đại" hơn một chút -- ít nhất, đó là giả định phổ biến. |

Tôi đoán nhiều người trong số các bạn ngày nay đã quen với việc nhìn thấy mã khung thành phần (React, v.v.) phần nào giống như thế này:

```jsx
// ..

infoForm(props) {
    return (
        <form ref={this.theFormElem}>
            <button type=submit onClick=this.clickHandler>
                Click Me
            </button>
        </form>
    );
}

// ..
```

Tất nhiên, có rất nhiều cách khác mà mã có thể được định hình, tùy thuộc vào việc bạn đang sử dụng khung này hay khung khác, v.v.

Hoặc có thể bạn thậm chí không sử dụng các thành phần kiểu `class` / `this` nữa, bởi vì bạn đã chuyển mọi thứ sang hooks và closures. Trong mọi trường hợp, cho mục đích thảo luận của chúng ta, chương *này* là tất cả về `this`, vì vậy chúng ta cần tuân thủ một phong cách mã hóa như trên, để có mã liên quan đến cuộc thảo luận.

Và cả hai đoạn mã trước đó đều không hiển thị hàm `clickHandler` đang được định nghĩa. Nhưng tôi đã nói nhiều lần cho đến nay, điều đó không quan trọng; tất cả những gì quan trọng là ... cái gì? hãy nói cùng tôi... tất cả những gì quan trọng là *cách* hàm được gọi.

Vậy `clickHandler` đang được gọi như thế nào? Vị trí gọi là gì, và nó khớp với quy tắc gán ngữ cảnh nào?

### Ẩn Khỏi Tầm Nhìn (Hidden From Sight)

Nếu bạn bị mắc kẹt, đừng lo lắng. Tôi cố tình làm cho điều này trở nên khó khăn, để chỉ ra một điều rất quan trọng.

Khi các ràng buộc trình xử lý sự kiện `"click"` hoặc `onClick=` xảy ra, trong cả hai trường hợp, chúng ta đã chỉ định `this.clickHandler`, điều này ngụ ý rằng có một đối tượng ngữ cảnh `this` với một thuộc tính trên nó được gọi là `clickHandler`, đang giữ định nghĩa hàm của chúng ta.

Vậy, `this.clickHandler` có phải là vị trí gọi không? Nếu phải, quy tắc gán nào áp dụng? Quy tắc *ngữ cảnh ngầm định* (#3)?

Thật không may, không.

Vấn đề là, **chúng ta không thể thực sự nhìn thấy vị trí gọi** trong chương trình này. Ồ ồ.

Nếu chúng ta không thể nhìn thấy vị trí gọi, làm thế nào chúng ta biết *cách* hàm thực sự sẽ được gọi?

*Đó* chính xác là điểm tôi đang đưa ra.

Không quan trọng là chúng ta đã truyền vào `this.clickHandler`. Đó chỉ đơn thuần là một tham chiếu đến một giá trị đối tượng hàm. Nó không phải là một vị trí gọi.

Bên dưới lớp vỏ, ở đâu đó bên trong một khung, thư viện, hoặc thậm chí chính môi trường JS, khi người dùng nhấp vào nút, một tham chiếu đến hàm `clickHandler(..)` sẽ được gọi. Và như chúng ta đã ngụ ý, vị trí gọi đó thậm chí sẽ truyền vào đối tượng sự kiện DOM làm đối số `evt`.

Vì chúng ta không thể nhìn thấy vị trí gọi, chúng ta phải *tưởng tượng* nó. Nó có thể trông giống như...?

```js
// ..
eventCallback( domEventObj );
// ..
```

Nếu đúng như vậy, quy tắc `this` nào sẽ áp dụng? Quy tắc *ngữ cảnh mặc định* (#4)?

Hoặc, nếu vị trí gọi trông giống như thế này thì sao...?

```js
// ..
eventCallback.call( domElement, domEventObj );
```

Bây giờ quy tắc `this` nào sẽ áp dụng? Quy tắc *ngữ cảnh rõ ràng* (#2)?

Trừ khi bạn mở và xem mã nguồn cho khung/thư viện, hoặc đọc tài liệu/đặc tả, bạn sẽ không *biết* những gì mong đợi ở vị trí gọi đó. Điều đó có nghĩa là việc dự đoán, cuối cùng, `this` trỏ đến cái gì trong hàm `clickHandler` bạn viết, là... nói một cách nhẹ nhàng... hơi phức tạp.

### *Cái Này* Sai Rồi (*This* Is Wrong)

Để giúp bạn bớt đau đớn hơn ở đây, tôi sẽ đi thẳng vào vấn đề.

Hầu như tất cả các triển khai của cơ chế xử lý nhấp chuột sẽ làm điều gì đó giống như `.call(..)`, và chúng sẽ đặt phần tử DOM (ví dụ: nút) mà trình lắng nghe sự kiện bị ràng buộc, làm *ngữ cảnh rõ ràng* cho lệnh gọi.

Hmmm... điều đó có ổn không, hay nó sẽ là một vấn đề?

Hãy nhớ lại rằng hàm `clickHandler(..)` của chúng ta là nhận biết `this`, và tham chiếu `this.theFormElem` của nó ngụ ý tham chiếu đến một đối tượng có thuộc tính `theFormElem`, đến lượt nó đang trỏ vào phần tử `<form>` cha. Các nút DOM, theo mặc định, không có thuộc tính `theFormElem` trên chúng.

Nói cách khác, tham chiếu `this` mà trình xử lý sự kiện của chúng ta sẽ được thiết lập cho nó gần như chắc chắn là sai. Rất tiếc.

Trừ khi chúng ta muốn viết lại hàm `clickHandler`, chúng ta sẽ cần phải sửa lỗi đó.

### Sửa `this`

Hãy xem xét một số tùy chọn để giải quyết việc gán sai. Để giữ cho mọi thứ tập trung, tôi sẽ tuân thủ phong cách ràng buộc sự kiện này cho cuộc thảo luận:

```js
this.submitBtnaddEventListener(
    "click",
    this.clickHandler,
    false
);
```

Đây là một cách để giải quyết nó:

```js
// lưu trữ một tham chiếu cố định đến ngữ cảnh
// `this` hiện tại
var context = this;

this.submitBtn.addEventListener(
    "click",
    function handler(evt){
        return context.clickHandler(evt);
    },
    false
);
```

| MẸO: |
| :--- |
| Hầu hết mã JS cũ hơn sử dụng cách tiếp cận này sẽ nói điều gì đó như `var self = this` thay vì tên `context` mà tôi đang đặt cho nó ở đây. "Self" là một từ ngắn hơn, và nghe có vẻ ngầu hơn. Nhưng nó cũng hoàn toàn sai về ý nghĩa ngữ nghĩa. Từ khóa `this` không phải là một tham chiếu "self" (bản thân) đến hàm, mà là ngữ cảnh cho lệnh gọi hàm hiện tại đó. Những thứ đó thoạt nhìn có vẻ giống nhau, nhưng chúng là những khái niệm hoàn toàn khác nhau, khác nhau như táo và một bài hát của Beatles. Vì vậy... để diễn giải lại chúng, "Này nhà phát triển, đừng làm cho nó tồi tệ. Hãy lấy một `self` buồn và làm cho nó trở thành `context` tốt hơn." |

Chuyện gì đang xảy ra ở đây? Tôi nhận ra rằng mã bao quanh, nơi lệnh gọi `addEventListener` sẽ chạy, có một ngữ cảnh `this` hiện tại là chính xác, và chúng ta cần đảm bảo rằng cùng một ngữ cảnh `this` đó được áp dụng khi `clickHandler(..)` được gọi.

Tôi đã định nghĩa một hàm bao quanh (`handler(..)`) và sau đó buộc vị trí gọi trông giống như:

```js
context.clickHandler(evt);
```

| MẸO: |
| :--- |
| Quy tắc gán ngữ cảnh `this` nào được áp dụng ở đây? Đúng vậy, quy tắc *ngữ cảnh ngầm định* (#3). |

Bây giờ, không quan trọng vị trí gọi nội bộ của thư viện/khung/môi trường trông như thế nào. Nhưng, tại sao?

Bởi vì bây giờ chúng ta *thực sự* đang kiểm soát vị trí gọi. Không quan trọng `handler(..)` được gọi như thế nào, hoặc `this` của nó được gán là gì. Chỉ quan trọng là khi `clickHandler(..)` được gọi, ngữ cảnh `this` được đặt thành những gì chúng ta muốn.

Tôi đã thực hiện thủ thuật đó không chỉ bằng cách định nghĩa một hàm bao quanh (`handler(..)`) để tôi có thể kiểm soát vị trí gọi, mà còn... và điều này quan trọng, vì vậy đừng bỏ lỡ nó... Tôi đã định nghĩa `handler(..)` là một hàm KHÔNG nhận biết `this`! Không có từ khóa `this` bên trong `handler(..)`, vì vậy bất kỳ `this` nào được thiết lập (hoặc không) bởi thư viện/khung/môi trường, đều hoàn toàn không liên quan.

Dòng `var context = this` rất quan trọng đối với thủ thuật. Nó định nghĩa một biến từ vựng `context`, không phải là một từ khóa đặc biệt nào đó, giữ một bản chụp nhanh của giá trị trong `this` bên ngoài. Sau đó bên trong `clickHandler`, chúng ta chỉ đơn thuần tham chiếu một biến từ vựng (`context`), không có từ khóa `this` tương đối/ma thuật nào.

### `this` Từ vựng (Lexical This)

Nhân tiện, tên cho mẫu này là "`this` từ vựng", có nghĩa là một `this` hoạt động giống như một biến phạm vi từ vựng thay vì giống như một ràng buộc ngữ cảnh động.

Nhưng hóa ra JS có một cách dễ dàng hơn để thực hiện trò ảo thuật "`this` từ vựng". Bạn đã sẵn sàng cho việc tiết lộ thủ thuật chưa!?

...

Hàm mũi tên `=>`! Tada!

Đúng vậy, hàm `=>`, không giống như tất cả các dạng hàm khác, là đặc biệt, ở chỗ nó hoàn toàn không đặc biệt. Hoặc, đúng hơn, rằng nó không định nghĩa bất cứ điều gì đặc biệt cho hành vi `this` cả.

Trong một hàm `=>`, từ khóa `this`... **không phải là một từ khóa**. Nó hoàn toàn không khác gì bất kỳ biến nào khác, như `context` hoặc `happyFace` hoặc `foobarbaz`.

Hãy để tôi minh họa quan điểm *này* trực tiếp hơn:

```js
function outer() {
    console.log(this.value);

    // định nghĩa và trả về một hàm "inner"
    var inner = () => {
        console.log(this.value);
    };

    return inner;
}

var one = {
    value: 42,
};
var two = {
    value: "buồn bã",
};

var innerFn = outer.call(one);
// 42

innerFn.call(two);
// 42   <-- không phải "buồn bã"
```

`innerFn.call(two)` sẽ, đối với bất kỳ định nghĩa hàm *thông thường* nào, dẫn đến `"buồn bã"` ở đây. Nhưng vì hàm `inner` mà chúng ta đã định nghĩa và trả về (và gán cho `innerFn`) là một hàm mũi tên `=>` *bất thường*, nó không có hành vi `this` đặc biệt, mà thay vào đó có hành vi "`this` từ vựng".

Khi hàm `innerFn(..)` (hay còn gọi là `inner(..)`) được gọi, ngay cả với một gán *ngữ cảnh rõ ràng* thông qua `.call(..)`, gán đó sẽ bị bỏ qua.

| LƯU Ý: |
| :--- |
| Tôi không chắc tại sao các hàm mũi tên `=>` thậm chí có `call(..)` / `apply(..)` trên chúng, vì chúng là các hàm no-op im lặng. Tôi đoán đó là để nhất quán với các hàm bình thường. Nhưng như chúng ta sẽ thấy sau này, có những sự không nhất quán khác giữa các hàm *thông thường* và các hàm mũi tên `=>` *bất thường*. |

Khi một `this` được bắt gặp (`this.value`) bên trong một hàm mũi tên `=>`, `this` được coi như một biến từ vựng bình thường, không phải là một từ khóa đặc biệt. Và vì không có biến `this` trong chính hàm đó, JS làm những gì nó luôn làm với các biến từ vựng: nó đi lên một cấp độ phạm vi từ vựng -- trong trường hợp này, đến hàm `outer(..)` bao quanh, và nó kiểm tra xem có bất kỳ `this` nào được đăng ký trong phạm vi đó không.

May mắn thay, `outer(..)` là một hàm *thông thường*, có nghĩa là nó có một từ khóa `this` bình thường. Và lệnh gọi `outer.call(one)` đã gán `one` cho `this` của nó.

Vì vậy, `innerFn.call(two)` đang gọi `inner()`, nhưng khi `inner()` tra cứu một giá trị cho `this`, nó nhận được... `one`, không phải `two`.

#### Quay Lại Với... Nút Bấm

Bạn đã nghĩ rằng tôi sẽ thực hiện một trò đùa chơi chữ và nói "tương lai" ở đó, phải không!?

Một cách trực tiếp và thích hợp hơn để giải quyết vấn đề trước đó của chúng ta, nơi chúng ta đã thực hiện `var context = this` để có được một loại hành vi "`this` từ vựng" giả mạo, là sử dụng hàm mũi tên `=>`, vì tính năng thiết kế chính của nó là... "`this` từ vựng".

```js
this.submitBtn.addEventListener(
    "click",
    evt => this.clickHandler(evt),
    false
);
```

Bùm! Vấn đề đã được giải quyết! Mic drop!

Hãy nghe tôi về điều *này*: hàm mũi tên `=>` *không phải* -- tôi nhắc lại, *không phải* -- là về việc gõ ít ký tự hơn. Điểm chính của hàm `=>` được thêm vào JS là cung cấp cho chúng ta hành vi "`this` từ vựng" mà không cần phải dùng đến các thủ thuật kiểu `var context = this` (hoặc tệ hơn, `var self = this`).

| MẸO: |
| :--- |
| Nếu bạn cần "`this` từ vựng", hãy luôn ưu tiên một hàm mũi tên `=>`. Nếu bạn không cần "`this` từ vựng", chà... hàm mũi tên `=>` có thể không phải là công cụ tốt nhất cho công việc. |

#### Thời Gian Thú Tội (Confession Time)

Tôi đã nói suốt trong chương này, rằng cách bạn viết một hàm, và nơi bạn viết hàm, *không liên quan gì* đến cách `this` của nó sẽ được gán.

Đối với các hàm thông thường, điều đó đúng. Nhưng khi chúng ta xem xét một hàm mũi tên `=>` bất thường, nó không còn hoàn toàn chính xác nữa.

Hãy nhớ lại dạng `=>` ban đầu của `clickHandler` từ đầu chương?

```js
const clickHandler = evt =>
    evt.target.matches("button") ?
        this.theFormElem.submit() :
        evt.stopPropagation();
```

Nếu chúng ta sử dụng dạng đó, trong cùng ngữ cảnh với ràng buộc sự kiện của chúng ta, nó có thể trông giống như thế này:

```js
const clickHandler = evt =>
    evt.target.matches("button") ?
        this.theFormElem.submit() :
        evt.stopPropagation();

this.submitBtn.addEventListener("click",clickHandler,false);
```

Rất nhiều nhà phát triển thích giảm nó hơn nữa, thành một hàm mũi tên `=>` nội tuyến:

```js
this.submitBtn.addEventListener(
    "click",
    evt => evt.target.matches("button") ?
        this.theFormElem.submit() :
        evt.stopPropagation(),
    false
);
```

Khi chúng ta viết một hàm mũi tên `=>`, chúng ta biết chắc chắn rằng ràng buộc `this` của nó sẽ chính xác là ràng buộc `this` hiện tại của bất kỳ hàm bao quanh nào đang chạy, bất kể vị trí gọi của hàm mũi tên `=>` trông như thế nào. Vì vậy, nói cách khác, *cách* chúng ta viết hàm mũi tên `=>`, và *nơi* chúng ta viết nó, có quan trọng.

Tuy nhiên, điều đó không trả lời đầy đủ câu hỏi `this`. Nó chỉ chuyển câu hỏi sang *cách hàm bao quanh được gọi*. Thực ra, sự tập trung vào vị trí gọi vẫn là điều duy nhất quan trọng.

Nhưng sắc thái mà tôi thú nhận đã bỏ qua cho đến thời điểm *này* là: quan trọng là vị trí gọi *nào* chúng ta xem xét, không chỉ *bất kỳ* vị trí gọi nào trong ngăn xếp cuộc gọi hiện tại. Vị trí gọi quan trọng là, lệnh gọi hàm gần nhất trong ngăn xếp cuộc gọi hiện tại ***thực sự gán một ngữ cảnh `this`***.

Vì một hàm mũi tên `=>` không bao giờ có một vị trí gọi gán `this` (bất kể thế nào), vị trí gọi đó không liên quan đến câu hỏi. Chúng ta phải tiếp tục bước lên ngăn xếp cuộc gọi cho đến khi chúng ta tìm thấy một lệnh gọi hàm *có* gán `this` -- ngay cả khi hàm được gọi đó bản thân nó không nhận biết `this`.

**ĐÓ** là vị trí gọi duy nhất quan trọng.

#### Tìm Vị Trí Gọi Đúng

Hãy để tôi minh họa, với một mớ hỗn độn phức tạp của một loạt các hàm/cuộc gọi lồng nhau:

```js
globalThis.value = { result: "Sad face" };

function one() {
    function two() {
        var three = {
            value: { result: "Hmmm" },

            fn: () => {
                const four = () => this.value;
                return four.call({
                    value: { result: "OK", },
                });
            },
        };
        return three.fn();
    };
    return two();
}

new one();          // ???
```

Bạn có thể chạy qua (cơn ác mộng) đó trong đầu và xác định những gì sẽ được trả về từ lệnh gọi `new one()` không?

Nó có thể là bất kỳ cái nào trong số này:

```js
// từ `four.call(..)`:
{ result: "OK" }

// hoặc, từ đối tượng `three`:
{ result: "Hmmm" }

// hoặc, từ `globalThis.value`:
{ result: "Sad face" }

// hoặc, đối tượng rỗng từ lệnh gọi `new`:

Ngăn xếp cuộc gọi (call-stack) cho lệnh gọi `new one()` đó là:

```
four         |
three.fn     |
two          | (this = globalThis)
one          | (this = {})
[ global ]   | (this = globalThis)
```

Vì `four()` và `fn()` đều là các hàm mũi tên `=>`, các vị trí gọi `three.fn()` và `four.call(..)` không phải là gán `this`; do đó, chúng không liên quan đến truy vấn của chúng ta. Lệnh gọi tiếp theo cần xem xét trong ngăn xếp cuộc gọi là gì? `two()`. Đó là một hàm thông thường (nó có thể chấp nhận gán `this`), và vị trí gọi khớp với quy tắc gán *ngữ cảnh mặc định* (#4). Vì chúng ta không ở chế độ nghiêm ngặt, `this` được gán là `globalThis`.

Khi `four()` đang chạy, `this` chỉ là một biến bình thường. Sau đó, nó nhìn vào hàm chứa nó (`three.fn()`), nhưng nó lại tìm thấy một hàm không có `this`. Vì vậy, nó đi lên một cấp độ khác, và tìm thấy một hàm *thông thường* `two()` có định nghĩa `this`. Và `this` đó là `globalThis`. Vì vậy, biểu thức `this.value` phân giải thành `globalThis.value`, trả về cho chúng ta... `{ result: "Sad face" }`.

...

Hít một hơi thật sâu. Tôi biết đó là rất nhiều thứ để xử lý trong đầu. Và công bằng mà nói, đó là một ví dụ siêu gượng ép. Bạn sẽ gần như không bao giờ thấy tất cả sự phức tạp đó trộn lẫn trong một ngăn xếp cuộc gọi.

Nhưng bạn hoàn toàn sẽ tìm thấy các ngăn xếp cuộc gọi hỗn hợp trong các chương trình thực tế. Bạn cần phải thoải mái với phân tích mà tôi vừa minh họa, để có thể gỡ bỏ ngăn xếp cuộc gọi cho đến khi bạn tìm thấy vị trí gọi gán `this` gần đây nhất.

Hãy nhớ câu ngạn ngữ tôi đã trích dẫn trước đó: "sức mạnh lớn đi kèm với trách nhiệm lớn". Chọn mã định hướng `this` (thậm chí là các `class`) có nghĩa là chọn cả sự linh hoạt mà nó mang lại cho chúng ta, cũng như cần phải thoải mái điều hướng ngăn xếp cuộc gọi để hiểu cách nó sẽ hoạt động.

Đó là cách duy nhất để viết (và sau này đọc!) mã nhận biết `this` một cách hiệu quả.

### Điều Này Chắc Chắn Sẽ Xảy Ra (This Is Bound To Come Up)

Quay lại một chút, có một tùy chọn khác nếu bạn không muốn sử dụng hành vi "`this` từ vựng" của hàm mũi tên `=>` để giải quyết chức năng trình xử lý sự kiện nút.

Ngoài `call(..)` / `apply(..)` -- hãy nhớ rằng những thứ này gọi hàm! -- Các hàm JS cũng có một tiện ích thứ ba được tích hợp sẵn, gọi là `bind(..)` -- cái mà *không* gọi hàm, chỉ để làm rõ.

Tiện ích `bind(..)` định nghĩa một phiên bản được bao bọc/ràng buộc *mới* của một hàm, trong đó `this` của nó được thiết lập trước và cố định, và không thể bị ghi đè bằng `call(..)` hoặc `apply(..)`, hoặc thậm chí là một đối tượng *ngữ cảnh ngầm định* tại vị trí gọi:

```js
this.submitBtn.addEventListener(
    "click",
    this.clickHandler.bind(this),
    false
);
```

Vì tôi đang truyền vào một hàm bị ràng buộc `this` làm trình xử lý sự kiện, nên tương tự như vậy, không quan trọng tiện ích đó cố gắng đặt `this` như thế nào, bởi vì tôi đã buộc `this` phải là những gì tôi muốn: giá trị của `this` từ ngữ cảnh gọi hàm bao quanh.

#### Hầu Như Không Mới (Hardly New)

Mẫu này thường được gọi là "ràng buộc cứng" (hard binding), vì chúng ta đang tạo một tham chiếu hàm được ràng buộc mạnh mẽ với một `this` cụ thể. Rất nhiều bài viết về JS đã tuyên bố rằng hàm mũi tên `=>` về cơ bản chỉ là cú pháp cho ràng buộc cứng `bind(this)`. Không phải vậy. Hãy cùng tìm hiểu.

Nếu bạn định tạo một tiện ích `bind(..)`, nó có thể trông giống như *thế này*:

```js
function bind(fn,context) {
    return function bound(...args){
        return fn.apply(context,args);
    };
}
```

| LƯU Ý: |
| :--- |
| Đây không thực sự là cách `bind(..)` được triển khai. Hành vi phức tạp và tinh tế hơn. Tôi chỉ minh họa một phần hành vi của nó trong đoạn mã này. |

Điều đó có vẻ quen thuộc không? Nó đang sử dụng thủ thuật "`this` từ vựng" giả mạo cũ kỹ. Và bên dưới lớp vỏ, đó là một gán *ngữ cảnh rõ ràng*, trong trường hợp này là thông qua `apply(..)`.

Vì vậy, chờ đã... điều đó không có nghĩa là chúng ta chỉ có thể làm điều đó với một hàm mũi tên `=>` sao?

```js
function bind(fn,context) {
    return (...args) => fn.apply(context,args);
}
```

Eh... không hẳn. Như với hầu hết mọi thứ trong JS, có một chút sắc thái. Hãy để tôi minh họa:

```js
// triển khai ứng cử viên, để so sánh
function fakeBind(fn,context) {
    return (...args) => fn.apply(context,args);
}

// đối tượng thử nghiệm
function thisAwareFn() {
    console.log(`Value: ${this.value}`);
}

// dữ liệu kiểm soát
var obj = {
    value: 42,
};

// thí nghiệm
var f = thisAwareFn.bind(obj);
var g = fakeBind(thisAwareFn,obj);

f();            // Value: 42
g();            // Value: 42

new f();        // Value: undefined
new g();        // <--- ???
```

Đầu tiên, hãy nhìn vào lệnh gọi `new f()`. Phải thừa nhận rằng đó là một cách sử dụng kỳ lạ, khi gọi `new` trên một hàm bị ràng buộc cứng. Có lẽ khá hiếm khi bạn làm như vậy. Nhưng nó cho thấy một điều gì đó khá thú vị. Mặc dù `f()` đã bị ràng buộc cứng với ngữ cảnh `this` của `obj`, toán tử `new` vẫn có thể chiếm quyền điều khiển `this` của hàm bị ràng buộc cứng và liên kết lại nó với đối tượng mới được tạo và rỗng. Đối tượng đó không có thuộc tính `value`, đó là lý do tại sao chúng ta thấy `"Value: undefined"` được in ra.

Nếu điều đó cảm thấy kỳ lạ, tôi đồng ý. Đó là một sắc thái góc kỳ lạ. Đó không phải là thứ bạn có thể sẽ khai thác. Nhưng tôi chỉ ra điều đó không chỉ vì chuyện vặt vãnh. Hãy tham khảo lại bốn quy tắc được trình bày trước đó trong chương này. Hãy nhớ cách tôi khẳng định thứ tự ưu tiên của chúng, và `new` đứng đầu (#1), trước quy tắc gán *rõ ràng* `call(..)` / `apply(..)` (#2)?

Vì chúng ta có thể nghĩ về `bind(..)` như một biến thể của quy tắc đó, bây giờ chúng ta thấy thứ tự ưu tiên đó đã được chứng minh. `new` được ưu tiên hơn, và có thể ghi đè, ngay cả một hàm bị ràng buộc cứng. Đại loại làm cho bạn nghĩ rằng hàm bị ràng buộc cứng có lẽ không bị ràng buộc "cứng" đến thế, hả?!

Nhưng... điều gì sẽ xảy ra với lệnh gọi `new g()`, đang gọi `new` trên hàm mũi tên `=>` được trả về? Bạn có dự đoán kết quả tương tự như `new f()` không?

Xin lỗi vì đã làm bạn thất vọng.

Dòng đó thực sự sẽ ném ra một ngoại lệ, bởi vì một hàm `=>` không thể được sử dụng với từ khóa `new`.

Nhưng tại sao? Câu trả lời tốt nhất của tôi, không phải là người có thẩm quyền về TC39, là về mặt khái niệm và thực tế, một hàm mũi tên `=>` không phải là một hàm có `this` bị ràng buộc cứng, nó là một hàm hoàn toàn không có `this`. Như vậy, `new` không có ý nghĩa gì đối với một hàm như vậy, vì vậy JS chỉ đơn giản là không cho phép nó.

| LƯU Ý: |
| :--- |
| Hãy nhớ lại trước đó, khi tôi đã chỉ ra rằng các hàm mũi tên `=>` có `call(..)`, `apply(..)`, và thực sự thậm chí là `bind(..)`. Nhưng chúng ta đã thấy rằng các hàm như vậy về cơ bản bỏ qua các tiện ích này như là no-ops (không hoạt động). Theo tôi, hơi lạ khi các hàm mũi tên `=>` có tất cả các tiện ích đó như là no-ops chuyển qua, nhưng đối với từ khóa `new`, đó không chỉ là, một lần nữa, một no-op chuyển qua, mà thay vào đó bị cấm với một ngoại lệ. |

Nhưng điểm chính là: một hàm mũi tên `=>` *không phải* là một dạng cú pháp của `bind(this)`.

### Thua Trận Chiến Này (Losing This Battle)

Quay trở lại một lần nữa với ví dụ trình xử lý sự kiện nút của chúng ta:

```js
this.submitBtnaddEventListener(
    "click",
    this.clickHandler,
    false
);
```

Có một mối quan tâm sâu sắc hơn mà chúng ta chưa giải quyết.

Chúng ta đã thấy một số cách tiếp cận khác nhau để xây dựng một tham chiếu hàm callback khác để truyền vào đó, thay thế cho `this.clickHandler`.

Nhưng bất kể chúng ta chọn cách nào trong số đó, chúng đều tạo ra một hàm hoàn toàn khác theo nghĩa đen, không chỉ là một sửa đổi tại chỗ đối với hàm `clickHandler` hiện có của chúng ta.

Tại sao điều đó lại quan trọng?

Chà, trước hết, chúng ta càng tạo ra nhiều hàm (và tạo lại), chúng ta càng tiêu tốn nhiều thời gian xử lý (rất nhỏ) và nhiều bộ nhớ (khá nhỏ, thường là vậy). Và khi chúng ta tạo lại một tham chiếu hàm, và vứt bỏ một cái cũ đi, điều đó cũng để lại bộ nhớ chưa được thu hồi nằm xung quanh, gây áp lực lên bộ thu gom rác (GC) để thường xuyên hơn, tạm dừng vũ trụ của chương trình của chúng ta trong giây lát trong khi nó dọn dẹp và thu hồi bộ nhớ đó.

Nếu việc kết nối lắng nghe sự kiện này là một hoạt động một lần, thì không có vấn đề gì lớn. Nhưng nếu nó xảy ra lặp đi lặp lại, các hiệu ứng hiệu suất cấp hệ thống *có thể* bắt đầu cộng dồn. Đã bao giờ có một hoạt ảnh trơn tru bị giật chưa? Đó có lẽ là do GC khởi động, dọn dẹp một loạt bộ nhớ có thể thu hồi.

Nhưng một mối quan tâm khác là, đối với những thứ như trình xử lý sự kiện, nếu chúng ta định xóa một trình lắng nghe sự kiện vào một thời điểm nào đó sau này, chúng ta cần giữ một tham chiếu đến chính xác cùng một hàm mà chúng ta đã đính kèm ban đầu. Nếu chúng ta đang sử dụng một thư viện/khung, thường (nhưng không phải luôn luôn!) chúng sẽ lo liệu chi tiết công việc bẩn thỉu nhỏ đó cho bạn. Nhưng nếu không, chúng ta phải đảm bảo rằng bất kỳ hàm nào chúng ta định đính kèm, chúng ta giữ một tham chiếu đề phòng trường hợp chúng ta cần nó sau này.

Vì vậy, quan điểm tôi đang đưa ra là: thiết lập trước một gán `this`, bất kể bạn làm điều đó như thế nào, để nó có thể dự đoán được, đều đi kèm với một chi phí. Một chi phí cấp hệ thống và một chi phí bảo trì/phức tạp chương trình. Nó *không bao giờ* miễn phí.

Một cách phản ứng với thực tế đó là quyết định, OK, chúng ta sẽ chỉ sản xuất tất cả các tham chiếu hàm được gán `this` đó một lần, trước thời hạn, ngay từ đầu. Bằng cách đó, chúng ta chắc chắn sẽ giảm cả áp lực hệ thống và áp lực mã xuống mức tối thiểu.

Nghe có vẻ hợp lý, phải không? Không nhanh thế đâu.

#### Ràng Buộc Trước Ngữ Cảnh Hàm (Pre-Binding Function Contexts)

Nếu bạn có một tham chiếu hàm một lần cần được ràng buộc `this`, và bạn sử dụng một mũi tên `=>` hoặc một lệnh gọi `bind(this)`, tôi không thấy bất kỳ vấn đề nào với điều đó.

Nhưng nếu hầu hết hoặc tất cả các hàm nhận biết `this` trong một phân đoạn mã của bạn được gọi theo những cách mà `this` không phải là ngữ cảnh có thể dự đoán được mà bạn mong đợi, và vì vậy bạn quyết định cần phải ràng buộc cứng tất cả chúng... Tôi nghĩ đó là một tín hiệu cảnh báo lớn rằng bạn đang đi sai hướng.

Vui lòng nhớ lại cuộc thảo luận trong phần "Tránh Điều Này" từ Chương 3, bắt đầu với đoạn mã này:

```js
class Point2d {
    x = null
    getDoubleX = () => this.x * 2

    constructor(x,y) {
        this.x = x;
        this.y = y;
    }
    toString() { /* .. */ }
}

var point = new Point2d(3,4);
```

Bây giờ hãy tưởng tượng chúng ta đã làm điều này với mã đó:

```js
const getX = point.getDoubleX;

// sau đó, ở nơi khác

getX();         // 6
```

Như bạn có thể thấy, vấn đề chúng ta đang cố gắng giải quyết giống như vấn đề chúng ta đã giải quyết ở đây trong chương này. Đó là chúng ta muốn có thể gọi một tham chiếu hàm như `getX()`, và để nó *có nghĩa* và *hoạt động giống như* `point.getDoubleX()`. Nhưng các quy tắc `this` trên các hàm *thông thường* không hoạt động theo cách đó.

Vì vậy, chúng ta đã sử dụng một hàm mũi tên `=>`. Không có vấn đề gì lớn, phải không!?

Sai.

Vấn đề gốc rễ thực sự là chúng ta *muốn* hai điều mâu thuẫn từ mã của mình, và chúng ta đang cố gắng sử dụng cùng một *cây búa* cho cả hai *cây đinh*.

Chúng ta muốn có một phương thức nhận biết `this` được lưu trữ trên nguyên mẫu `class`, để chỉ có một định nghĩa cho hàm, và tất cả các lớp con và thể hiện của chúng ta chia sẻ độc đáo cùng một hàm đó. Và cách tất cả chúng chia sẻ là thông qua sức mạnh của ràng buộc `this` động.

Nhưng đồng thời, chúng ta *cũng* muốn các tham chiếu hàm đó duy trì được gán `this` một cách kỳ diệu cho thể hiện của chúng ta khi chúng ta truyền các tham chiếu hàm đó xung quanh và mã khác chịu trách nhiệm về vị trí gọi.

Nói cách khác, đôi khi chúng ta muốn một cái gì đó như `point.getDoubleX` có nghĩa là, "cho tôi một tham chiếu được gán `this` cho `point`", và những lần khác chúng ta muốn cùng một biểu thức `point.getDoubleX` có nghĩa là, cho tôi một tham chiếu hàm có thể gán `this` động để nó có thể nhận ngữ cảnh tôi cần vào lúc này.

Có lẽ JS có thể cung cấp một toán tử khác ngoài `.`, như `::` hoặc `->` hoặc một cái gì đó tương tự, cho phép bạn phân biệt loại tham chiếu hàm nào bạn đang theo đuổi. Trên thực tế, có một đề xuất lâu dài cho một toán tử ràng buộc `this` (`::`), thu hút sự chú ý theo thời gian, và sau đó dường như bị đình trệ. Ai biết được, có thể một ngày nào đó một toán tử như vậy cuối cùng sẽ hạ cánh, và chúng ta sẽ có các tùy chọn tốt hơn.

Nhưng tôi thực sự nghi ngờ rằng ngay cả khi nó hạ cánh vào một ngày nào đó, nó sẽ bán một tham chiếu hàm hoàn toàn mới, chính xác như các cách tiếp cận `=>` hoặc `bind(this)` mà chúng ta đã nói đến. Nó sẽ không đến như một giải pháp miễn phí và hoàn hảo. Sẽ luôn có một sự căng thẳng giữa việc muốn cùng một hàm đôi khi linh hoạt `this` và đôi khi có thể dự đoán `this`.

Những gì các tác giả JS của mã định hướng `class` thường gặp phải, sớm hay muộn, chính là sự căng thẳng này. Và bạn biết họ làm gì không?

Họ không xem xét *chi phí* của việc chỉ đơn giản là ràng buộc trước tất cả các phương thức nhận biết `this` của lớp thay vì là các hàm mũi tên `=>` trong các thuộc tính thành viên. Họ không nhận ra rằng nó hoàn toàn đánh bại toàn bộ mục đích của chuỗi `[[Prototype]]`. Và họ không nhận ra rằng nếu ngữ cảnh cố định là những gì họ *thực sự cần*, thì có một cơ chế hoàn toàn khác trong JS phù hợp hơn cho mục đích đó.

### Hãy Nhìn Nhận Một Cách Phê Bình Hơn (Take A More Critical Look)

Vì vậy, khi bạn làm loại điều này:

```js
class Point2d {
    x = null
    y = null
    getDoubleX = () => this.x * 2
    toString = () => `(${this.x},${this.y})`

    constructor(x,y) {
        this.x = x;
        this.y = y;
    }
}

var point = new Point2d(3,4);
var anotherPoint = new Point2d(5,6);

var f = point.getDoubleX;
var g = anotherPoint.toString;

f();            // 6
g();            // (5,6)
```

Tôi nói, "kinh!", đối với các phương thức nhận biết `this` bị ràng buộc cứng `getDoubleX()` và `toString()` ở đó. Đối với tôi, đó là một mùi mã (code smell). Nhưng đây là một cách tiếp cận thậm chí còn *tệ hơn* đã được nhiều nhà phát triển ưa chuộng trong quá khứ:

```js
class Point2d {
    x = null
    y = null

    constructor(x,y) {
        this.x = x;
        this.y = y;
        this.getDoubleX = this.getDoubleX.bind(this);
        this.toString = this.toString.bind(this);
    }
    getDoubleX() { return this.x * 2; }
    toString() { return `(${this.x},${this.y})`; }
}

var point = new Point2d(3,4);
var anotherPoint = new Point2d(5,6);

var f = point.getDoubleX;
var g = anotherPoint.toString;

f();            // 6
g();            // (5,6)
```

Kinh gấp đôi.

Trong cả hai trường hợp, bạn đang sử dụng cơ chế `this` nhưng hoàn toàn phản bội/vô hiệu hóa nó, bằng cách lấy đi tất cả sự năng động mạnh mẽ của `this`.

Bạn thực sự ít nhất nên xem xét cách tiếp cận thay thế này, bỏ qua hoàn toàn cơ chế `this`:

```js
function Point2d(px,py) {
    var x = px;
    var y = py;

    return {
        getDoubleX() { return x * 2; },
        toString() { return `(${x},${y})`; }
    };
}

var point = Point2d(3,4);
var anotherPoint = Point2d(5,6);

var f = point.getDoubleX;
var g = anotherPoint.toString;

f();            // 6
g();            // (5,6)
```

Bạn thấy không? Không có `this` xấu xí hoặc phức tạp nào làm lộn xộn mã đó hoặc phải lo lắng về các trường hợp góc. Phạm vi từ vựng cực kỳ đơn giản và trực quan.

Khi tất cả những gì chúng ta muốn là hầu hết/tất cả các hành vi hàm của chúng ta có ngữ cảnh cố định và có thể dự đoán được, giải pháp thích hợp nhất, giải pháp đơn giản nhất và thậm chí hiệu quả nhất, là các biến từ vựng và đóng phạm vi (scope closure).

Khi bạn đi đến tất cả những rắc rối của việc rắc các tham chiếu `this` lên khắp một đoạn mã, và sau đó bạn cắt bỏ toàn bộ cơ chế ở đầu gối bằng `=>` "`this` từ vựng" hoặc `bind(this)`, bạn đã chọn làm cho mã dài dòng hơn, phức tạp hơn, quá mức cần thiết. Và bạn không nhận được gì từ nó có lợi hơn, ngoại trừ việc đi theo trào lưu `this` (và `class`).

...

Hít thở sâu. Thu thập lại bản thân.

Tôi đang nói chuyện với chính mình, không phải bạn. Nhưng nếu những gì tôi vừa nói làm phiền bạn, tôi cũng đang nói chuyện với bạn!

OK, nghe này. Đó chỉ là ý kiến của tôi. Nếu bạn không đồng ý, điều đó ổn thôi. Nhưng hãy áp dụng cùng mức độ nghiêm ngặt để suy nghĩ về cách các cơ chế này hoạt động, như tôi đã làm, khi bạn quyết định kết luận nào bạn muốn đi đến.

## Các Biến Thể (Variations)

Trước khi chúng ta kết thúc cuộc thảo luận dài dòng về `this`, có một vài biến thể bất thường về các lệnh gọi hàm mà chúng ta nên thảo luận.

### Gọi Hàm Gián Tiếp (Indirect Function Calls)

Hãy nhớ lại ví dụ này từ đầu chương?

```js
var point = {
    x: null,
    y: null,

    init(x,y) {
        this.x = x;
        this.y = y;
    },
    rotate(angleRadians) { /* .. */ },
    toString() { /* .. */ },
};

var init = point.init;
init(3,4);                  // hỏng!
```

Điều này bị hỏng vì vị trí gọi `init(3,4)` không cung cấp tín hiệu gán `this` cần thiết. Nhưng có những cách khác để quan sát sự cố tương tự. Ví dụ:

```js
(1,point.init)(3,4);        // hỏng!
```

Cú pháp trông kỳ lạ này trước tiên đang đánh giá một biểu thức `(1,point.init)`, là một biểu thức chuỗi dấu phẩy. Kết quả của một biểu thức như vậy là giá trị được đánh giá cuối cùng, trong trường hợp này là tham chiếu hàm (được giữ bởi `point.init`).

Vì vậy, kết quả đặt tham chiếu hàm đó vào ngăn xếp biểu thức, và sau đó gọi giá trị đó với `(3,4)`. Đó là một lệnh gọi gián tiếp của hàm. Và kết quả là gì? Nó thực sự khớp với quy tắc gán *ngữ cảnh mặc định* (#4) mà chúng ta đã xem xét trước đó trong chương.

Do đó, trong chế độ không nghiêm ngặt, `this` cho lệnh gọi `point.init(..)` sẽ là `globalThis`. Nếu chúng ta ở chế độ nghiêm ngặt, nó sẽ là `undefined`, và thao tác `this.x = x` sau đó sẽ ném ra một ngoại lệ vì truy cập không hợp lệ vào thuộc tính `x` trên giá trị `undefined`.

Có một số cách khác nhau để có được một lệnh gọi hàm gián tiếp. Ví dụ:

```js
(()=>point.init)()(3,4);    // hỏng!
```

Và một ví dụ khác về gọi hàm gián tiếp là mẫu Biểu thức Hàm Được Gọi Ngay Lập Tức (IIFE):

```js
(function(){
    // `this` được gán thông qua quy tắc "mặc định"
})();
```

Như bạn có thể thấy, giá trị biểu thức hàm được đưa vào ngăn xếp biểu thức, và sau đó nó được gọi với `()` ở cuối.

Nhưng còn mã này thì sao:

```js
(point.init)(3,4);
```

Kết quả của mã đó sẽ là gì?

Theo cùng một lý luận mà chúng ta đã thấy trong các ví dụ trước, có lý do để cho rằng biểu thức `point.init` đặt giá trị hàm vào ngăn xếp biểu thức, và sau đó được gọi gián tiếp với `(3,4)`.

Tuy nhiên, không hẳn vậy! Ngữ pháp JS có một quy tắc đặc biệt để xử lý dạng gọi `(someIdentifier)(..)` như thể nó là `someIdentifier(..)` (không có `(..)` xung quanh tên định danh).

Tự hỏi tại sao bạn có thể muốn buộc *ngữ cảnh mặc định* cho việc gán `this` thông qua một lệnh gọi hàm gián tiếp?

### Truy Cập `globalThis`

Trước khi chúng ta trả lời điều đó, hãy giới thiệu một cách khác để thực hiện gán `this` hàm gián tiếp. Cho đến nay, các mẫu gọi hàm gián tiếp được hiển thị đều nhạy cảm với chế độ nghiêm ngặt. Nhưng nếu chúng ta muốn một gán `this` hàm gián tiếp không tôn trọng chế độ nghiêm ngặt thì sao.

Hàm tạo `Function(..)` lấy một chuỗi mã và định nghĩa động hàm tương đương. Tuy nhiên, nó luôn làm như vậy như thể hàm đó đã được khai báo trong phạm vi toàn cục. Và hơn nữa, nó đảm bảo hàm như vậy *không* chạy trong chế độ nghiêm ngặt, bất kể trạng thái chế độ nghiêm ngặt của chương trình. Đó là kết quả tương tự như chạy một gián tiếp

Một cách sử dụng thích hợp của việc gán `this` hàm gián tiếp bất khả tri chế độ nghiêm ngặt như vậy là để có được một tham chiếu đáng tin cậy đến đối tượng toàn cục thực sự trước khi đặc tả JS thực sự định nghĩa định danh `globalThis` (ví dụ, trong một polyfill cho nó):

```js
"use strict";

var gt = new Function("return this")();
gt === globalThis;                      // true
```

Trên thực tế, một kết quả tương tự, sử dụng thủ thuật toán tử dấu phẩy (xem phần trước) và `eval(..)`:

```js
"use strict";

function getGlobalThis() {
    return (1,eval)("this");
}

getGlobalThis() === globalThis;      // true
```

| LƯU Ý: |
| :--- |
| `eval("this")` sẽ nhạy cảm với chế độ nghiêm ngặt, nhưng `(1,eval)("this")` thì không, và do đó cung cấp cho chúng ta `globalThis` một cách đáng tin cậy trong bất kỳ chương trình nào. |

Thật không may, cả hai cách tiếp cận `new Function(..)` và `(1,eval)(..)` đều có một hạn chế quan trọng: mã đó sẽ bị chặn trong mã JS dựa trên trình duyệt nếu ứng dụng được phục vụ với một số hạn chế Chính sách Bảo mật Nội dung (CSP) nhất định, không cho phép đánh giá mã động (vì lý do bảo mật).

Chúng ta có thể giải quyết vấn đề này không? Có, hầu hết. [^globalThisPolyfill]

Đặc tả JS nói rằng một hàm getter được định nghĩa trên đối tượng toàn cục, hoặc trên bất kỳ đối tượng nào kế thừa từ nó (như `Object.prototype`), chạy hàm getter với ngữ cảnh `this` được gán cho `globalThis`, bất kể chế độ nghiêm ngặt của chương trình.

```js
// Được điều chỉnh từ: https://mathiasbynens.be/notes/globalthis#robust-polyfill
function getGlobalThis() {
    Object.defineProperty(Object.prototype,"__get_globalthis__",{
        get() { return this; },
        configurable: true
    });
    var gt = __get_globalthis__;
    delete Object.prototype.__get_globalthis__;
    return gt;
}

getGlobalThis() === globalThis;      // true
```

Vâng, điều đó thật siêu phức tạp. Nhưng đó là `this` của JS dành cho bạn!

### Hàm Thẻ Mẫu (Template Tag Functions)

Có thêm một biến thể bất thường của việc gọi hàm mà chúng ta nên đề cập: các hàm mẫu được gắn thẻ (tagged template functions).

Chuỗi mẫu (template strings) -- cái mà tôi thích gọi là các literal nội suy -- có thể được "gắn thẻ" với một hàm tiền tố, được gọi với nội dung đã phân tích của template literal:

```js
function tagFn(/* .. */) {
    // ..
}

tagFn`actually a function invocation!`;
```

Như bạn có thể thấy, không có cú pháp gọi `(..)`, chỉ có hàm thẻ (`tagFn`) xuất hiện trước `` `template literal` ``; khoảng trắng được phép giữa chúng, nhưng rất không phổ biến.

Mặc dù có vẻ ngoài kỳ lạ, hàm `tagFn(..)` sẽ được gọi. Nó được truyền danh sách một hoặc nhiều chuỗi literal đã được phân tích từ template literal, cùng với bất kỳ giá trị biểu thức nội suy nào đã gặp phải.

Chúng ta sẽ không đề cập đến tất cả các chi tiết của các hàm mẫu được gắn thẻ -- chúng thực sự là một trong những tính năng mạnh mẽ và thú vị nhất từng được thêm vào JS -- nhưng vì chúng ta đang nói về việc gán `this` trong các lệnh gọi hàm, để đầy đủ, chúng ta cần nói về cách `this` sẽ được gán.

Dạng khác cho các hàm thẻ mà bạn có thể gặp phải là:

```js
var someObj = {
    tagFn() { /* .. */ }
};

someObj.tagFn`also a function invocation!`;
```

Đây là lời giải thích dễ dàng: `` tagFn`..` `` và `` someObj.tagFn`..` `` mỗi cái sẽ có hành vi gán `this` tương ứng với các vị trí gọi như `tagFn(..)` và `someObj.tagFn(..)`, tương ứng. Nói cách khác, `` tagFn`..` `` hoạt động theo quy tắc gán *ngữ cảnh mặc định* (#4), và `` someObj.tagFn`..` `` hoạt động theo quy tắc gán *ngữ cảnh ngầm định* (#3).

May mắn cho chúng ta, chúng ta không cần phải lo lắng về các quy tắc gán `new` hoặc `call(..)` / `apply(..)`, vì các dạng đó không thể thực hiện được với các hàm thẻ.

Cần phải chỉ ra rằng khá hiếm khi một hàm template literal được gắn thẻ được định nghĩa là nhận biết `this`, vì vậy khá khó có khả năng bạn sẽ cần áp dụng các quy tắc này. Nhưng đề phòng trường hợp, bây giờ bạn đã *biết*.

## Luôn Nhận Biết (Stay Aware)

Vậy đó, đó là `this`. Tôi sẵn sàng cá rằng đối với nhiều người trong số các bạn, nó có một chút... chúng ta nên nói là, liên quan... hơn những gì bạn có thể mong đợi.

Tin tốt, có lẽ, là trong thực tế bạn không thường xuyên vấp phải tất cả những phức tạp khác nhau này. Nhưng bạn càng sử dụng `this`, nó càng đòi hỏi bạn, và những người đọc mã của bạn, phải hiểu cách nó thực sự hoạt động.

Bài học ở đây là bạn nên có chủ ý và nhận thức về tất cả các khía cạnh của `this` trước khi bạn rắc nó vào mã của mình. Hãy chắc chắn rằng bạn đang sử dụng nó hiệu quả nhất và tận dụng tối đa trụ cột quan trọng này của JS.

[^globalThisPolyfill]: "A horrifying globalThis polyfill in universal JavaScript"; Mathias Bynens; April 18 2019; https://mathiasbynens.be/notes/globalthis#robust-polyfill ; Accessed July 2022
