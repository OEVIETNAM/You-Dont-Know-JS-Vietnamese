---
layout: default
title: Chương 5
parent: Phạm Vi & Closures
nav_order: 6
---

# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Chương 5: Vòng Đời (Không Hề) Bí Mật Của Biến

Đến bây giờ bạn nên có một sự nắm bắt tốt về sự lồng nhau của các phạm vi, từ phạm vi toàn cục xuống—được gọi là chuỗi phạm vi của chương trình.

Nhưng chỉ biết phạm vi nào mà một biến đến từ đó chỉ là một phần của câu chuyện. Nếu một khai báo biến xuất hiện sau câu lệnh đầu tiên của một phạm vi, bất kỳ tham chiếu nào đến định danh đó *trước* khai báo sẽ hoạt động như thế nào? Điều gì xảy ra nếu bạn cố gắng khai báo cùng một biến hai lần trong một phạm vi?

Hương vị đặc biệt của JS về phạm vi từ vựng rất phong phú với sắc thái trong cách và khi nào các biến xuất hiện và trở nên có sẵn cho chương trình.

## Khi Nào Tôi Có Thể Sử Dụng Một Biến?

Tại thời điểm nào một biến trở nên có sẵn để sử dụng trong phạm vi của nó? Có vẻ như có một câu trả lời rõ ràng: *sau khi* biến đã được khai báo/tạo. Đúng không? Không hẳn.

Hãy xem xét:

```js
greeting();
// Hello!

function greeting() {
    console.log("Hello!");
}
```

Mã này hoạt động tốt. Bạn có thể đã thấy hoặc thậm chí viết mã như thế này trước đây. Nhưng bạn đã bao giờ tự hỏi làm thế nào hoặc tại sao nó hoạt động không? Cụ thể, tại sao bạn có thể truy cập định danh `greeting` từ dòng 1 (để truy xuất và thực thi một tham chiếu hàm), mặc dù khai báo hàm `greeting()` không xảy ra cho đến dòng 4?

Nhớ lại Chương 1 chỉ ra rằng tất cả các định danh được đăng ký vào các phạm vi tương ứng của chúng trong thời gian biên dịch. Hơn nữa, mỗi định danh được *tạo* ở đầu phạm vi mà nó thuộc về, **mỗi khi phạm vi đó được nhập**.

Thuật ngữ thường được sử dụng nhất cho một biến có thể nhìn thấy từ đầu phạm vi bao quanh của nó, mặc dù khai báo của nó có thể xuất hiện xa hơn trong phạm vi, được gọi là **hoisting**.

Nhưng chỉ hoisting thôi không trả lời đầy đủ câu hỏi. Chúng ta có thể thấy một định danh có tên `greeting` từ đầu phạm vi, nhưng tại sao chúng ta có thể **gọi** hàm `greeting()` trước khi nó được khai báo?

Nói cách khác, làm thế nào biến `greeting` có bất kỳ giá trị nào (tham chiếu hàm) được gán cho nó, từ thời điểm phạm vi bắt đầu chạy? Câu trả lời là một đặc điểm đặc biệt của các khai báo `function` chính thức, được gọi là *function hoisting*. Khi định danh tên của khai báo `function` được đăng ký ở đầu phạm vi của nó, nó cũng được tự động khởi tạo thành tham chiếu của hàm đó. Đó là lý do tại sao hàm có thể được gọi trong toàn bộ phạm vi!

Một chi tiết quan trọng là cả *function hoisting* và *variable hoisting* hương vị `var` đều gắn định danh tên của chúng vào **phạm vi hàm** bao quanh gần nhất (hoặc, nếu không có, phạm vi toàn cục), không phải phạm vi khối.

| LƯU Ý: |
| :--- |
| Các khai báo với `let` và `const` vẫn hoist (xem cuộc thảo luận TDZ sau trong chương này). Nhưng hai dạng khai báo này gắn vào khối bao quanh của chúng thay vì chỉ một hàm bao quanh như với các khai báo `var` và `function`. Xem "Phạm Vi với Khối" trong Chương 6 để biết thêm thông tin. |

### Hoisting: Khai Báo so với Biểu Thức

*Function hoisting* chỉ áp dụng cho các khai báo `function` chính thức (cụ thể là những cái xuất hiện bên ngoài các khối—xem "FiB" trong Chương 6), không phải cho các gán biểu thức `function`. Hãy xem xét:

```js
greeting();
// TypeError

var greeting = function greeting() {
    console.log("Hello!");
};
```

Dòng 1 (`greeting();`) ném ra một lỗi. Nhưng *loại* lỗi được ném ra rất quan trọng để chú ý. Một `TypeError` có nghĩa là chúng ta đang cố gắng làm điều gì đó với một giá trị không được phép. Tùy thuộc vào môi trường JS của bạn, thông báo lỗi sẽ nói điều gì đó như, "'undefined' is not a function," hoặc hữu ích hơn, "'greeting' is not a function."

Chú ý rằng lỗi **không phải** là `ReferenceError`. JS không nói với chúng ta rằng nó không thể tìm thấy `greeting` như một định danh trong phạm vi. Nó nói với chúng ta rằng `greeting` đã được tìm thấy nhưng không giữ một tham chiếu hàm tại thời điểm đó. Chỉ các hàm mới có thể được gọi, vì vậy cố gắng gọi một số giá trị không phải hàm dẫn đến lỗi.

Nhưng `greeting` giữ gì, nếu không phải tham chiếu hàm?

Ngoài việc được hoisted, các biến được khai báo với `var` cũng được tự động khởi tạo thành `undefined` ở đầu phạm vi của chúng—một lần nữa, hàm bao quanh gần nhất, hoặc toàn cục. Khi được khởi tạo, chúng có sẵn để được sử dụng (được gán cho, truy xuất từ, v.v.) trong toàn bộ phạm vi.

Vì vậy, trên dòng đầu tiên đó, `greeting` tồn tại, nhưng nó chỉ giữ giá trị `undefined` mặc định. Phải đến dòng 4 thì `greeting` mới được gán tham chiếu hàm.

Hãy chú ý kỹ đến sự phân biệt ở đây. Một khai báo `function` được hoisted **và được khởi tạo thành giá trị hàm của nó** (một lần nữa, được gọi là *function hoisting*). Một biến `var` cũng được hoisted, và sau đó được tự động khởi tạo thành `undefined`. Bất kỳ gán biểu thức `function` tiếp theo nào cho biến đó không xảy ra cho đến khi gán đó được xử lý trong quá trình thực thi thời gian chạy.

Trong cả hai trường hợp, tên của định danh được hoisted. Nhưng liên kết tham chiếu hàm không được xử lý tại thời gian khởi tạo (đầu phạm vi) trừ khi định danh được tạo trong một khai báo `function` chính thức.

### Variable Hoisting

Hãy xem một ví dụ khác về *variable hoisting*:

```js
greeting = "Hello!";
console.log(greeting);
// Hello!

var greeting = "Howdy!";
```

Mặc dù `greeting` không được khai báo cho đến dòng 5, nó có sẵn để được gán sớm nhất là dòng 1. Tại sao?

Có hai phần cần thiết cho lời giải thích:

* định danh được hoisted,
* **và** nó được tự động khởi tạo thành giá trị `undefined` từ đầu phạm vi.

| LƯU Ý: |
| :--- |
| Sử dụng *variable hoisting* loại này có lẽ cảm thấy không tự nhiên, và nhiều độc giả có thể đúng khi muốn tránh dựa vào nó trong các chương trình của họ. Nhưng liệu tất cả hoisting (bao gồm *function hoisting*) nên được tránh? Chúng ta sẽ khám phá những quan điểm khác nhau này về hoisting chi tiết hơn trong Phụ lục A. |

## Hoisting: Một Phép Ẩn Dụ Khác

Chương 2 đầy các phép ẩn dụ (để minh họa phạm vi), nhưng ở đây chúng ta phải đối mặt với một phép ẩn dụ khác: chính hoisting. Thay vì hoisting là một bước thực thi cụ thể mà công cụ JS thực hiện, nó hữu ích hơn khi nghĩ về hoisting như một hình dung của các hành động khác nhau mà JS thực hiện trong việc thiết lập chương trình **trước khi thực thi**.

Khẳng định điển hình về ý nghĩa của hoisting: *nâng lên*—như nâng một trọng lượng nặng lên—bất kỳ định danh nào lên đầu phạm vi. Lời giải thích thường được khẳng định là công cụ JS sẽ thực sự *viết lại* chương trình đó trước khi thực thi, để nó trông giống như thế này hơn:

```js
var greeting;           // khai báo được hoisted
greeting = "Hello!";    // dòng 1 ban đầu
console.log(greeting);  // Hello!
greeting = "Howdy!";    // `var` đã biến mất!
```

Phép ẩn dụ hoisting đề xuất rằng JS tiền xử lý chương trình ban đầu và sắp xếp lại nó một chút, để tất cả các khai báo đã được di chuyển lên đầu các phạm vi tương ứng của chúng, trước khi thực thi. Hơn nữa, phép ẩn dụ hoisting khẳng định rằng các khai báo `function` được, trong toàn bộ của chúng, hoisted lên đầu mỗi phạm vi. Hãy xem xét:

```js
studentName = "Suzy";
greeting();
// Hello Suzy!

function greeting() {
    console.log(`Hello ${ studentName }!`);
}
var studentName;
```

"Quy tắc" của phép ẩn dụ hoisting là các khai báo hàm được hoisted trước, sau đó các biến được hoisted ngay sau tất cả các hàm. Do đó, câu chuyện hoisting gợi ý rằng chương trình được *sắp xếp lại* bởi công cụ JS để trông như thế này:

```js
function greeting() {
    console.log(`Hello ${ studentName }!`);
}
var studentName;

studentName = "Suzy";
greeting();
// Hello Suzy!
```

Phép ẩn dụ hoisting này thuận tiện. Lợi ích của nó là cho phép chúng ta vẫy tay qua việc tiền xử lý nhìn trước kỳ diệu cần thiết để tìm tất cả các khai báo này được chôn sâu trong các phạm vi và bằng cách nào đó di chuyển (hoist) chúng lên đầu; chúng ta chỉ có thể nghĩ về chương trình như thể nó được thực thi bởi công cụ JS trong **một lần duyệt**, từ trên xuống dưới.

Một lần duyệt chắc chắn có vẻ đơn giản hơn khẳng định của Chương 1 về xử lý hai giai đoạn.

Hoisting như một cơ chế để sắp xếp lại mã có thể là một sự đơn giản hóa hấp dẫn, nhưng nó không chính xác. Công cụ JS không thực sự sắp xếp lại mã. Nó không thể nhìn trước một cách kỳ diệu và tìm các khai báo; cách duy nhất để tìm chúng một cách chính xác, cũng như tất cả các ranh giới phạm vi trong chương trình, sẽ là phân tích cú pháp đầy đủ mã.

Đoán xem phân tích cú pháp là gì? Giai đoạn đầu tiên của xử lý hai giai đoạn! Không có thể dục tinh thần kỳ diệu nào vượt qua sự thật đó.

Vì vậy, nếu phép ẩn dụ hoisting (tốt nhất) không chính xác, chúng ta nên làm gì với thuật ngữ? Tôi nghĩ nó vẫn hữu ích—thực sự, ngay cả các thành viên của TC39 thường xuyên sử dụng nó!—nhưng tôi không nghĩ chúng ta nên tuyên bố nó là một sắp xếp lại thực tế của mã nguồn.

| CẢNH BÁO: |
| :--- |
| Các mô hình tinh thần không chính xác hoặc không đầy đủ thường vẫn có vẻ đủ vì chúng đôi khi có thể dẫn đến câu trả lời đúng ngẫu nhiên. Nhưng về lâu dài, khó phân tích và dự đoán kết quả chính xác hơn nếu suy nghĩ của bạn không đặc biệt phù hợp với cách công cụ JS hoạt động. |

Tôi khẳng định rằng hoisting *nên* được sử dụng để chỉ **hoạt động thời gian biên dịch** của việc tạo các hướng dẫn thời gian chạy cho việc đăng ký tự động của một biến ở đầu phạm vi của nó, mỗi khi phạm vi đó được nhập.

Đó là một sự thay đổi tinh tế nhưng quan trọng, từ hoisting như một hành vi thời gian chạy đến vị trí thích hợp của nó trong số các nhiệm vụ thời gian biên dịch.

## Khai Báo Lại?

Bạn nghĩ điều gì xảy ra khi một biến được khai báo nhiều hơn một lần trong cùng một phạm vi? Hãy xem xét:

```js
var studentName = "Frank";
console.log(studentName);
// Frank

var studentName;
console.log(studentName);   // ???
```

Bạn mong đợi điều gì được in cho thông báo thứ hai đó? Nhiều người tin rằng `var studentName` thứ hai đã khai báo lại biến (và do đó "đặt lại" nó), vì vậy họ mong đợi `undefined` được in.

Nhưng có một thứ như một biến được "khai báo lại" trong cùng một phạm vi không? Không.

Nếu bạn xem xét chương trình này từ quan điểm của phép ẩn dụ hoisting, mã sẽ được sắp xếp lại như thế này cho mục đích thực thi:

```js
var studentName;
var studentName;    // rõ ràng là một no-op vô nghĩa!

studentName = "Frank";
console.log(studentName);
// Frank

console.log(studentName);
// Frank
```

Vì hoisting thực sự là về việc đăng ký một biến ở đầu phạm vi, không có gì để làm ở giữa phạm vi nơi chương trình ban đầu thực sự có câu lệnh `var studentName` thứ hai. Đó chỉ là một no-op(eration), một câu lệnh vô nghĩa.

| MẸO: |
| :--- |
| Theo phong cách của câu chuyện trò chuyện từ Chương 2, *Compiler* sẽ tìm thấy câu lệnh khai báo `var` thứ hai và hỏi *Scope Manager* nếu nó đã thấy một định danh `studentName`; vì nó đã có, sẽ không có gì khác để làm. |

Cũng quan trọng để chỉ ra rằng `var studentName;` không có nghĩa là `var studentName = undefined;`, như hầu hết cho là. Hãy chứng minh chúng khác nhau bằng cách xem xét biến thể này của chương trình:

```js
var studentName = "Frank";
console.log(studentName);   // Frank

var studentName;
console.log(studentName);   // Frank <--- vẫn!

// hãy thêm khởi tạo rõ ràng
var studentName = undefined;
console.log(studentName);   // undefined <--- thấy chưa!?
```

Thấy cách khởi tạo `= undefined` rõ ràng tạo ra kết quả khác với việc giả định nó xảy ra ngầm định khi bị bỏ qua không? Trong phần tiếp theo, chúng ta sẽ xem lại chủ đề khởi tạo các biến từ các khai báo của chúng.

Một khai báo `var` lặp lại của cùng tên định danh trong một phạm vi thực sự là một hoạt động không làm gì. Đây là một minh họa khác, lần này qua một hàm có cùng tên:

```js
var greeting;

function greeting() {
    console.log("Hello!");
}

// về cơ bản, một no-op
var greeting;

typeof greeting;        // "function"

var greeting = "Hello!";

typeof greeting;        // "string"
```

Khai báo `greeting` đầu tiên đăng ký định danh vào phạm vi, và vì nó là một `var` nên tự động khởi tạo sẽ là `undefined`. Khai báo `function` không cần đăng ký lại định danh, nhưng vì *function hoisting* nó ghi đè tự động khởi tạo để sử dụng tham chiếu hàm. `var greeting` thứ hai tự nó không làm gì vì `greeting` đã là một định danh và *function hoisting* đã ưu tiên cho tự động khởi tạo.

Thực sự gán `"Hello!"` cho `greeting` thay đổi giá trị của nó từ hàm `greeting()` ban đầu thành chuỗi; chính `var` không có bất kỳ hiệu ứng nào.

Còn lặp lại một khai báo trong một phạm vi bằng cách sử dụng `let` hoặc `const` thì sao?

```js
let studentName = "Frank";

console.log(studentName);

let studentName = "Suzy";
```

Chương trình này sẽ không thực thi, mà thay vào đó ngay lập tức ném ra một `SyntaxError`. Tùy thuộc vào môi trường JS của bạn, thông báo lỗi sẽ chỉ ra điều gì đó như: "studentName has already been declared." Nói cách khác, đây là một trường hợp mà cố gắng "khai báo lại" rõ ràng không được phép!

Không chỉ là hai khai báo liên quan đến `let` sẽ ném lỗi này. Nếu một trong hai khai báo sử dụng `let`, cái kia có thể là `let` hoặc `var`, và lỗi vẫn sẽ xảy ra, như được minh họa với hai biến thể này:

```js
var studentName = "Frank";

let studentName = "Suzy";
```

và:

```js
let studentName = "Frank";

var studentName = "Suzy";
```

Trong cả hai trường hợp, một `SyntaxError` được ném ra trên khai báo *thứ hai*. Nói cách khác, cách duy nhất để "khai báo lại" một biến là sử dụng `var` cho tất cả (hai hoặc nhiều) khai báo của nó.

Nhưng tại sao không cho phép nó? Lý do cho lỗi không phải là kỹ thuật per se, vì "khai báo lại" `var` luôn được phép; rõ ràng, cùng một sự cho phép có thể đã được thực hiện cho `let`.

Nó thực sự là một vấn đề "kỹ thuật xã hội" hơn. "Khai báo lại" các biến được một số người, bao gồm nhiều người trong cơ quan TC39, coi là một thói quen xấu có thể dẫn đến lỗi chương trình. Vì vậy, khi ES6 giới thiệu `let`, họ quyết định ngăn chặn "khai báo lại" với một lỗi.

| LƯU Ý: |
| :--- |
| Tất nhiên đây là một ý kiến phong cách, không thực sự là một lập luận kỹ thuật. Nhiều nhà phát triển đồng ý với vị trí này, và đó có lẽ là một phần lý do tại sao TC39 bao gồm lỗi (cũng như `let` tuân theo `const`). Nhưng một trường hợp hợp lý có thể đã được thực hiện rằng duy trì nhất quán với tiền lệ của `var` là thận trọng hơn, và rằng việc thực thi ý kiến như vậy tốt nhất được để lại cho các công cụ tham gia như linters. Trong Phụ lục A, chúng ta sẽ khám phá liệu `var` (và hành vi liên quan của nó, như "khai báo lại") vẫn có thể hữu ích trong JS hiện đại. |

Khi *Compiler* hỏi *Scope Manager* về một khai báo, nếu định danh đó đã được khai báo, và nếu một trong hai/cả hai khai báo được thực hiện với `let`, một lỗi được ném ra. Tín hiệu dự định cho nhà phát triển là "Ngừng dựa vào khai báo lại cẩu thả!"

### Hằng Số?

Từ khóa `const` bị hạn chế hơn `let`. Giống như `let`, `const` không thể được lặp lại với cùng định danh trong cùng một phạm vi. Nhưng thực sự có một lý do kỹ thuật ghi đè tại sao loại "khai báo lại" đó không được phép, không giống như `let` không cho phép "khai báo lại" chủ yếu vì lý do phong cách.

Từ khóa `const` yêu cầu một biến được khởi tạo, vì vậy bỏ qua một phép gán từ khai báo dẫn đến một `SyntaxError`:

```js
const empty;   // SyntaxError
```

Các khai báo `const` tạo ra các biến không thể được gán lại:

```js
const studentName = "Frank";
console.log(studentName);
// Frank

studentName = "Suzy";   // TypeError
```

Biến `studentName` không thể được gán lại vì nó được khai báo với một `const`.

| CẢNH BÁO: |
| :--- |
| Lỗi được ném ra khi gán lại `studentName` là một `TypeError`, không phải `SyntaxError`. Sự phân biệt tinh tế ở đây thực sự khá quan trọng, nhưng thật không may quá dễ bỏ lỡ. Lỗi cú pháp đại diện cho lỗi trong chương trình ngăn nó thậm chí bắt đầu thực thi. Lỗi kiểu đại diện cho lỗi phát sinh trong quá trình thực thi chương trình. Trong đoạn mã trước, `"Frank"` được in ra trước khi chúng ta xử lý gán lại `studentName`, sau đó ném ra lỗi. |

Vì vậy, nếu các khai báo `const` không thể được gán lại, và các khai báo `const` luôn yêu cầu gán, thì chúng ta có một lý do kỹ thuật rõ ràng tại sao `const` phải không cho phép bất kỳ "khai báo lại" nào: bất kỳ "khai báo lại" `const` nào cũng nhất thiết sẽ là một gán lại `const`, điều này không thể được phép!

```js
const studentName = "Frank";

// rõ ràng đây phải là một lỗi
const studentName = "Suzy";
```

Vì "khai báo lại" `const` phải không được phép (trên những cơ sở kỹ thuật đó), TC39 về cơ bản cảm thấy rằng "khai báo lại" `let` cũng nên không được phép, để nhất quán. Có thể tranh luận nếu đây là lựa chọn tốt nhất, nhưng ít nhất chúng ta có lý do đằng sau quyết định.

### Vòng Lặp

Vì vậy, rõ ràng từ cuộc thảo luận trước đó của chúng ta rằng JS không thực sự muốn chúng ta "khai báo lại" các biến của chúng ta trong cùng một phạm vi. Điều đó có lẽ có vẻ như một lời khuyên đơn giản, cho đến khi bạn xem xét ý nghĩa của nó đối với việc thực thi lặp lại các câu lệnh khai báo trong các vòng lặp. Hãy xem xét:

```js
var keepGoing = true;
while (keepGoing) {
    let value = Math.random();
    if (value > 0.5) {
        keepGoing = false;
    }
}
```

`value` có đang được "khai báo lại" lặp đi lặp lại trong chương trình này không? Chúng ta sẽ nhận được lỗi được ném ra không? Không.

Tất cả các quy tắc của phạm vi (bao gồm "khai báo lại" của các biến được tạo bằng `let`) được áp dụng *cho mỗi thể hiện phạm vi*. Nói cách khác, mỗi khi một phạm vi được nhập trong quá trình thực thi, mọi thứ đặt lại.

Mỗi lần lặp vòng lặp là thể hiện phạm vi mới riêng của nó, và trong mỗi thể hiện phạm vi, `value` chỉ được khai báo một lần. Vì vậy, không có cố gắng "khai báo lại", và do đó không có lỗi.

Trước khi chúng ta xem xét các dạng vòng lặp khác, điều gì sẽ xảy ra nếu khai báo `value` trong đoạn mã trước được thay đổi thành một `var`?

```js
var keepGoing = true;
while (keepGoing) {
    var value = Math.random();
    if (value > 0.5) {
        keepGoing = false;
    }
}
```

`value` có đang được "khai báo lại" ở đây không, đặc biệt là vì chúng ta biết `var` cho phép nó? Không. Bởi vì `var` không được coi là một khai báo phạm vi khối (xem Chương 6), nó gắn chính nó vào phạm vi toàn cục. Vì vậy, chỉ có một biến `value`, trong cùng phạm vi với `keepGoing` (phạm vi toàn cục, trong trường hợp này). Không có "khai báo lại" ở đây, cả!

Một cách để giữ tất cả điều này thẳng thắn là nhớ rằng các từ khóa `var`, `let`, và `const` thực sự bị *xóa* khỏi mã vào thời điểm nó bắt đầu thực thi. Chúng được xử lý hoàn toàn bởi trình biên dịch.

Nếu bạn xóa các từ khóa khai báo trong tâm trí và sau đó cố gắng xử lý mã, nó sẽ giúp bạn quyết định nếu và khi nào (khai báo lại) có thể xảy ra.

Còn "khai báo lại" với các dạng vòng lặp khác, như vòng lặp `for` thì sao?

```js
for (let i = 0; i < 3; i++) {
    let value = i * 10;
    console.log(`${ i }: ${ value }`);
}
// 0: 0
// 1: 10
// 2: 20
```

Nó nên rõ ràng rằng chỉ có một `value` được khai báo cho mỗi thể hiện phạm vi. Nhưng còn `i` thì sao? Nó có đang được "khai báo lại" không?

Để trả lời điều đó, hãy xem xét phạm vi nào mà `i` nằm trong. Có vẻ như nó sẽ nằm trong phạm vi bên ngoài (trong trường hợp này, toàn cục), nhưng nó không phải. Nó nằm trong phạm vi của thân vòng lặp `for`, giống như `value`. Trên thực tế, bạn có thể nghĩ về vòng lặp đó ở dạng tương đương dài dòng hơn này:

```js
{
    // một biến hư cấu để minh họa
    let $$i = 0;

    for ( /* nothing */; $$i < 3; $$i++) {
        // đây là `i` vòng lặp thực tế của chúng ta!
        let i = $$i;

        let value = i * 10;
        console.log(`${ i }: ${ value }`);
    }
    // 0: 0
    // 1: 10
    // 2: 20
}
```

Bây giờ nó nên rõ ràng: các biến `i` và `value` đều được khai báo chính xác một lần **cho mỗi thể hiện phạm vi**. Không có "khai báo lại" ở đây.

Còn các dạng vòng lặp `for` khác thì sao?

```js
for (let index in students) {
    // điều này tốt
}

for (let student of students) {
    // điều này cũng vậy
}
```

Điều tương tự với vòng lặp `for..in` và `for..of`: biến được khai báo được coi là *bên trong* thân vòng lặp, và do đó được xử lý cho mỗi lần lặp (hay còn gọi là, cho mỗi thể hiện phạm vi). Không có "khai báo lại."

OK, tôi biết bạn đang nghĩ rằng tôi nghe như một đĩa hát bị hỏng tại thời điểm này. Nhưng hãy khám phá cách `const` ảnh hưởng đến các cấu trúc vòng lặp này. Hãy xem xét:

```js
var keepGoing = true;
while (keepGoing) {
    // ooo, một hằng số sáng bóng!
    const value = Math.random();
    if (value > 0.5) {
        keepGoing = false;
    }
}
```

Giống như biến thể `let` của chương trình này mà chúng ta đã thấy trước đó, `const` đang được chạy chính xác một lần trong mỗi lần lặp vòng lặp, vì vậy nó an toàn khỏi rắc rối "khai báo lại". Nhưng mọi thứ trở nên phức tạp hơn khi chúng ta nói về vòng lặp `for`.

`for..in` và `for..of` tốt để sử dụng với `const`:

```js
for (const index in students) {
    // điều này tốt
}

for (const student of students) {
    // điều này cũng tốt
}
```

Nhưng không phải vòng lặp `for` chung:

```js
for (const i = 0; i < 3; i++) {
    // rất tiếc, điều này sẽ thất bại với
    // một Type Error sau lần lặp đầu tiên
}
```

Có gì sai ở đây? Chúng ta có thể sử dụng `let` tốt trong cấu trúc này, và chúng ta khẳng định rằng nó tạo ra một `i` mới cho mỗi phạm vi lần lặp vòng lặp, vì vậy nó thậm chí không có vẻ là một "khai báo lại."

Hãy "mở rộng" vòng lặp đó trong tâm trí như chúng ta đã làm trước đó:

```js
{
    // một biến hư cấu để minh họa
    const $$i = 0;

    for ( ; $$i < 3; $$i++) {
        // đây là `i` vòng lặp thực tế của chúng ta!
        const i = $$i;
        // ..
    }
}
```

Bạn có phát hiện vấn đề không? `i` của chúng ta thực sự chỉ được tạo một lần bên trong vòng lặp. Đó không phải là vấn đề. Vấn đề là `$$i` khái niệm phải được tăng mỗi lần với biểu thức `$$i++`. Đó là **gán lại** (không phải "khai báo lại"), điều này không được phép cho các hằng số.

Hãy nhớ, dạng "mở rộng" này chỉ là một mô hình khái niệm để giúp bạn trực giác nguồn gốc của vấn đề. Bạn có thể tự hỏi liệu JS có thể đã làm cho `const $$i = 0` thay vào đó thành `let $$i = 0`, sau đó sẽ cho phép `const` hoạt động với vòng lặp `for` cổ điển của chúng ta không? Có thể, nhưng sau đó nó có thể đã giới thiệu các ngoại lệ có khả năng đáng ngạc nhiên cho ngữ nghĩa vòng lặp `for`.

Ví dụ, nó sẽ là một ngoại lệ sắc thái khá tùy tiện (và có khả năng gây nhầm lẫn) để cho phép `i++` trong tiêu đề vòng lặp `for` tránh sự nghiêm ngặt của gán `const`, nhưng không cho phép các gán lại khác của `i` bên trong lần lặp vòng lặp, như đôi khi hữu ích.

Câu trả lời đơn giản là: `const` không thể được sử dụng với dạng vòng lặp `for` cổ điển vì gán lại bắt buộc.

Thú vị là, nếu bạn không thực hiện gán lại, thì nó hợp lệ:

```js
var keepGoing = true;

for (const i = 0; keepGoing; /* nothing here */ ) {
    keepGoing = (Math.random() > 0.5);
    // ..
}
```

Điều đó hoạt động, nhưng nó vô nghĩa. Không có lý do để khai báo `i` ở vị trí đó với một `const`, vì toàn bộ điểm của một biến như vậy ở vị trí đó là **được sử dụng để đếm các lần lặp**. Chỉ cần sử dụng một dạng vòng lặp khác, như vòng lặp `while`, hoặc sử dụng một `let`!

## Biến Chưa Được Khởi Tạo (hay còn gọi là TDZ)

Với các khai báo `var`, biến được "hoisted" lên đầu phạm vi của nó. Nhưng nó cũng được tự động khởi tạo thành giá trị `undefined`, để biến có thể được sử dụng trong toàn bộ phạm vi.

Tuy nhiên, các khai báo `let` và `const` không hoàn toàn giống nhau trong khía cạnh này.

Hãy xem xét:

```js
console.log(studentName);
// ReferenceError

let studentName = "Suzy";
```

Kết quả của chương trình này là một `ReferenceError` được ném ra trên dòng đầu tiên. Tùy thuộc vào môi trường JS của bạn, thông báo lỗi có thể nói điều gì đó như: "Cannot access studentName before initialization."

| LƯU Ý: |
| :--- |
| Thông báo lỗi như được thấy ở đây từng mơ hồ hoặc gây hiểu lầm hơn nhiều. Rất may, một số người trong số chúng ta trong cộng đồng đã có thể vận động thành công cho các công cụ JS cải thiện thông báo lỗi này để nó chính xác hơn nói với bạn điều gì sai! |

Thông báo lỗi đó khá chỉ ra những gì sai: `studentName` tồn tại trên dòng 1, nhưng nó chưa được khởi tạo, vì vậy nó không thể được sử dụng. Hãy thử điều này:

```js
studentName = "Suzy";   // hãy thử khởi tạo nó!
// ReferenceError

console.log(studentName);

let studentName;
```

Rất tiếc. Chúng ta vẫn nhận được `ReferenceError`, nhưng bây giờ trên dòng đầu tiên nơi chúng ta đang cố gắng gán cho (hay còn gọi là, khởi tạo!) biến "chưa được khởi tạo" này `studentName`. Chuyện gì đang xảy ra!?

Câu hỏi thực sự là, làm thế nào chúng ta khởi tạo một biến chưa được khởi tạo? Đối với `let`/`const`, cách **duy nhất** để làm như vậy là với một phép gán được gắn vào một câu lệnh khai báo. Một phép gán tự nó là không đủ! Hãy xem xét:

```js
let studentName = "Suzy";
console.log(studentName);   // Suzy
```

Ở đây, chúng ta đang khởi tạo `studentName` (trong trường hợp này, thành `"Suzy"` thay vì `undefined`) bằng cách dạng câu lệnh khai báo `let` được kết hợp với một phép gán.

Hoặc:

```js
// ..

let studentName;
// hoặc:
// let studentName = undefined;

// ..

studentName = "Suzy";

console.log(studentName);
// Suzy
```

| LƯU Ý: |
| :--- |
| Điều đó thú vị! Nhớ lại từ trước đó, chúng ta đã nói rằng `var studentName;` *không* giống với `var studentName = undefined;`, nhưng ở đây với `let`, chúng hoạt động giống nhau. Sự khác biệt xuất phát từ thực tế là `var studentName` tự động khởi tạo ở đầu phạm vi, trong khi `let studentName` thì không. |

Hãy nhớ rằng chúng ta đã khẳng định một vài lần cho đến nay rằng *Compiler* kết thúc bằng việc xóa bất kỳ khai báo `var`/`let`/`const` nào, thay thế chúng bằng các hướng dẫn ở đầu mỗi phạm vi để đăng ký các định danh thích hợp.

Vì vậy, nếu chúng ta phân tích những gì đang xảy ra ở đây, chúng ta thấy rằng một sắc thái bổ sung là *Compiler* cũng đang thêm một hướng dẫn ở giữa chương trình, tại điểm nơi biến `studentName` được khai báo, để xử lý tự động khởi tạo của khai báo đó. Chúng ta không thể sử dụng biến tại bất kỳ điểm nào trước khi tự động khởi tạo đó xảy ra. Điều tương tự cũng đúng cho `const` như nó làm cho `let`.

Thuật ngữ được đặt ra bởi TC39 để chỉ *khoảng thời gian* này từ việc nhập một phạm vi đến nơi tự động khởi tạo của biến xảy ra là: Vùng Chết Tạm thời (Temporal Dead Zone - TDZ).

TDZ là cửa sổ thời gian nơi một biến tồn tại nhưng vẫn chưa được khởi tạo, và do đó không thể được truy cập theo bất kỳ cách nào. Chỉ việc thực thi các hướng dẫn được để lại bởi *Compiler* tại điểm của khai báo ban đầu mới có thể thực hiện khởi tạo đó. Sau thời điểm đó, TDZ hoàn thành, và biến tự do được sử dụng cho phần còn lại của phạm vi.

Một `var` cũng về mặt kỹ thuật có một TDZ, nhưng nó có độ dài bằng không và do đó không thể quan sát được đối với các chương trình của chúng ta! Chỉ `let` và `const` có TDZ có thể quan sát được.

Nhân tiện, "temporal" (tạm thời) trong TDZ thực sự đề cập đến *thời gian* không phải *vị trí trong mã*. Hãy xem xét:

```js
askQuestion();
// ReferenceError

let studentName = "Suzy";

function askQuestion() {
    console.log(`${ studentName }, do you know?`);
}
```

Mặc dù về mặt vị trí `console.log(..)` tham chiếu `studentName` đến *sau* khai báo `let studentName`, về mặt thời gian hàm `askQuestion()` được gọi *trước* câu lệnh `let` được gặp, trong khi `studentName` vẫn đang trong TDZ của nó! Do đó lỗi.

Có một quan niệm sai lầm phổ biến rằng TDZ có nghĩa là `let` và `const` không hoist. Đây là một tuyên bố không chính xác, hoặc ít nhất hơi gây hiểu lầm. Chúng chắc chắn hoist.

Sự khác biệt thực sự là các khai báo `let`/`const` không tự động khởi tạo ở đầu phạm vi, theo cách `var` làm. *Cuộc tranh luận* sau đó là liệu tự động khởi tạo có phải là *một phần của* hoisting, hay không? Tôi nghĩ tự động đăng ký một biến ở đầu phạm vi (tức là, những gì tôi gọi là "hoisting") và tự động khởi tạo ở đầu phạm vi (thành `undefined`) là các hoạt động riêng biệt và không nên được gộp lại với nhau dưới thuật ngữ đơn lẻ "hoisting."

Chúng ta đã thấy rằng `let` và `const` không tự động khởi tạo ở đầu phạm vi. Nhưng hãy chứng minh rằng `let` và `const` *thực sự* hoist (tự động đăng ký ở đầu phạm vi), nhờ vào bạn của chúng ta shadowing (xem "Shadowing" trong Chương 3):

```js
var studentName = "Kyle";

{
    console.log(studentName);
    // ???

    // ..

    let studentName = "Suzy";

    console.log(studentName);
    // Suzy
}
```

Điều gì sẽ xảy ra với câu lệnh `console.log(..)` đầu tiên? Nếu `let studentName` không hoist lên đầu phạm vi, thì `console.log(..)` đầu tiên *nên* in `"Kyle"`, đúng không? Tại thời điểm đó, có vẻ như, chỉ có `studentName` bên ngoài tồn tại, vì vậy đó là biến mà `console.log(..)` nên truy cập và in.

Nhưng thay vào đó, `console.log(..)` đầu tiên ném ra một lỗi TDZ, bởi vì trên thực tế, `studentName` của phạm vi bên trong **đã** được hoisted (tự động đăng ký ở đầu phạm vi). Điều **không** xảy ra (chưa!) là tự động khởi tạo của `studentName` bên trong đó; nó vẫn chưa được khởi tạo tại thời điểm đó, do đó vi phạm TDZ!

Vì vậy, để tóm tắt, lỗi TDZ xảy ra vì các khai báo `let`/`const` *thực sự* hoist các khai báo của chúng lên đầu phạm vi của chúng, nhưng không giống như `var`, chúng hoãn tự động khởi tạo các biến của chúng cho đến thời điểm trong trình tự mã nơi khai báo ban đầu xuất hiện. Cửa sổ thời gian này (gợi ý: tạm thời), dù độ dài của nó là bao nhiêu, là TDZ.

Làm thế nào bạn có thể tránh lỗi TDZ?

Lời khuyên của tôi: luôn đặt các khai báo `let` và `const` của bạn ở đầu bất kỳ phạm vi nào. Thu nhỏ cửa sổ TDZ xuống độ dài bằng không (hoặc gần bằng không), và sau đó nó sẽ không còn vấn đề.

Nhưng tại sao TDZ lại là một thứ? Tại sao TC39 không ra lệnh rằng `let`/`const` tự động khởi tạo theo cách `var` làm? Chỉ cần kiên nhẫn, chúng ta sẽ quay lại khám phá *tại sao* của TDZ trong Phụ lục A.

## Cuối Cùng Được Khởi Tạo

Làm việc với các biến có nhiều sắc thái hơn vẻ ngoài ban đầu. *Hoisting*, *(khai báo lại)*, và *TDZ* là các nguồn gây nhầm lẫn phổ biến cho các nhà phát triển, đặc biệt là những người đã làm việc với các ngôn ngữ khác trước khi đến với JS. Trước khi tiếp tục, hãy đảm bảo mô hình tinh thần của bạn được căn cứ đầy đủ trên các khía cạnh này của phạm vi và biến JS.

Hoisting thường được trích dẫn như một cơ chế rõ ràng của công cụ JS, nhưng nó thực sự là một phép ẩn dụ hơn để mô tả các cách khác nhau mà JS xử lý các khai báo biến trong quá trình biên dịch. Nhưng ngay cả như một phép ẩn dụ, hoisting cung cấp cấu trúc hữu ích để suy nghĩ về vòng đời của một biến—khi nó được tạo, khi nó có sẵn để sử dụng, khi nó biến mất.

Khai báo và khai báo lại các biến có xu hướng gây nhầm lẫn khi được coi là các hoạt động thời gian chạy. Nhưng nếu bạn chuyển sang suy nghĩ thời gian biên dịch cho các hoạt động này, các quirks và *shadows* giảm bớt.

Lỗi TDZ (vùng chết tạm thời) là kỳ lạ và bực bội khi gặp phải. Rất may, TDZ tương đối đơn giản để tránh nếu bạn luôn cẩn thận đặt các khai báo `let`/`const` ở đầu bất kỳ phạm vi nào.

Khi bạn điều hướng thành công những khúc quanh này của phạm vi biến, chương tiếp theo sẽ đặt ra các yếu tố hướng dẫn quyết định của chúng ta để đặt các khai báo của chúng ta trong các phạm vi khác nhau, đặc biệt là các khối lồng nhau.
