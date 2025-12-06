---
layout: default
title: Chương 1
parent: Kiểu & Ngữ pháp
nav_order: 2
---

# Chương 1: Các Giá Trị Nguyên Thủy

| LƯU Ý: |
| :--- |
| Đang thực hiện |

Trong Chương 1 của cuốn sách "Đối Tượng & Các Lớp" thuộc bộ sách này, chúng ta đã đối mặt với quan niệm sai lầm phổ biến rằng "mọi thứ trong JS đều là đối tượng". Bây giờ chúng ta quay lại chủ đề đó, và một lần nữa xua tan huyền thoại đó.

Ở đây, chúng ta sẽ xem xét các kiểu giá trị cốt lõi của JS, đặc biệt là các kiểu không phải đối tượng được gọi là *nguyên thủy* (primitives).

## Các Kiểu Giá Trị

JS không áp dụng các kiểu cho các biến hoặc thuộc tính -- cái mà tôi gọi là "các kiểu chứa" (container types) -- mà thay vào đó, chính các giá trị có các kiểu -- cái mà tôi gọi là "các kiểu giá trị" (value types).

Ngôn ngữ cung cấp bảy kiểu giá trị nguyên thủy (không phải đối tượng) được tích hợp sẵn: [^PrimitiveValues]

* `undefined`
* `null`
* `boolean`
* `number`
* `bigint`
* `symbol`
* `string`

Các kiểu giá trị này định nghĩa các tập hợp của một hoặc nhiều giá trị cụ thể, mỗi tập hợp có một bộ các hành vi chung cho tất cả các giá trị của mỗi kiểu.

### Type-Of

Kiểu giá trị của bất kỳ giá trị nào cũng có thể được kiểm tra thông qua toán tử `typeof`, toán tử này luôn trả về một giá trị `string` đại diện cho kiểu giá trị JS cơ bản:

```js
typeof true;            // "boolean"

typeof 42;              // "number"

typeof 42n;             // "bigint"

typeof Symbol("42");    // "symbol"
```

Toán tử `typeof`, khi được sử dụng đối với một biến thay vì một giá trị, sẽ báo cáo kiểu giá trị của *giá trị trong biến đó*:

```js
greeting = "Hello";
typeof greeting;        // "string"
```

Bản thân các biến JS không có kiểu. Chúng giữ bất kỳ giá trị tùy ý nào, mà bản thân giá trị đó có một kiểu giá trị.

### Không phải đối tượng?

Điều gì cụ thể làm cho 7 kiểu giá trị nguyên thủy khác biệt với các kiểu giá trị đối tượng (và các kiểu phụ)? Tại sao chúng ta không coi tất cả chúng về cơ bản là *đối tượng* ở bên dưới?

Hãy xem xét:

```js
myName = "Kyle";

myName.nickname = "getify";

console.log(myName.nickname);           // undefined
```

Đoạn mã này có vẻ âm thầm thất bại trong việc thêm thuộc tính `nickname` vào một chuỗi nguyên thủy. Nhìn bề ngoài, điều đó có thể ngụ ý rằng các nguyên thủy thực sự chỉ là các đối tượng ở bên dưới, như nhiều người đã (nhầm lẫn) khẳng định trong nhiều năm qua.

| CẢNH BÁO: |
| :--- |
| Người ta có thể giải thích sự thất bại âm thầm đó là một ví dụ về *auto-boxing* (xem "Các Đối Tượng Tự Động" trong Chương 3), nơi nguyên thủy được chuyển đổi ngầm định thành một thể hiện đối tượng bao bọc `String` trong khi cố gắng gán thuộc tính, và sau đó đối tượng nội bộ này bị vứt bỏ sau khi câu lệnh hoàn thành. Trên thực tế, tôi đã nói chính xác như vậy trong ấn bản đầu tiên của cuốn sách này. Nhưng tôi đã sai; thật đáng tiếc! |

Một cái gì đó sâu sắc hơn đang diễn ra, như chúng ta thấy trong phiên bản này của đoạn trích trước:

```js
"use strict";

myName = "Kyle";

myName.nickname = "getify";
// TypeError: Cannot create property 'nickname'
// on string 'Kyle'
```

Thú vị thật! Trong chế độ nghiêm ngặt (strict-mode), JS thực thi một hạn chế không cho phép thiết lập một thuộc tính mới trên một giá trị nguyên thủy, như thể ngầm định thăng cấp nó thành một đối tượng mới.

Ngược lại, trong chế độ không nghiêm ngặt, JS cho phép vi phạm diễn ra mà không bị nhắc đến. Vậy tại sao? Bởi vì chế độ nghiêm ngặt đã được thêm vào ngôn ngữ trong ES5.1 (2011), hơn 15 năm sau, và một sự thay đổi như vậy sẽ phá vỡ các chương trình hiện có nếu nó không được định nghĩa là nhạy cảm với khai báo chế độ nghiêm ngặt mới.

Vậy chúng ta có thể kết luận gì về sự phân biệt giữa các nguyên thủy và các đối tượng? Các nguyên thủy là các giá trị *không được phép có các thuộc tính*; chỉ các đối tượng mới được phép như vậy.

| MẸO: |
| :--- |
| Sự phân biệt cụ thể này dường như mâu thuẫn với các biểu thức như `"hello".length`; ngay cả trong chế độ nghiêm ngặt, nó trả về giá trị mong đợi `5`. Vì vậy, chắc chắn *có vẻ* như chuỗi có thuộc tính `length`! Nhưng, như vừa đề cập trước đó, lời giải thích chính xác là *auto-boxing*; chúng ta sẽ đề cập đến chủ đề này trong "Các Đối Tượng Tự Động" ở Chương 3. |

## Các Giá Trị Rỗng

Các kiểu `null` và `undefined` đều thường đại diện cho một sự trống rỗng hoặc vắng mặt của giá trị.

Thật không may, kiểu giá trị `null` có một kết quả `typeof` không mong đợi. Thay vì `"null"`, chúng ta thấy:

```js
typeof null;            // "object"
```

Không, điều đó không có nghĩa là `null` bằng cách nào đó là một loại đối tượng đặc biệt. Nó chỉ là một di sản của những ngày đầu của JS, cái mà không thể thay đổi vì nó sẽ phá vỡ rất nhiều mã ngoài kia.

Kiểu `undefined` được báo cáo cho cả các giá trị `undefined` rõ ràng và bất kỳ nơi nào gặp phải một giá trị dường như bị thiếu:

```js
typeof undefined;               // "undefined"

var whatever;

typeof whatever;                // "undefined"
typeof nonExistent;             // "undefined"

whatever = {};
typeof whatever.missingProp;    // "undefined"

whatever = [];
typeof whatever[10];            // "undefined"
```

| LƯU Ý: |
| :--- |
| Biểu thức `typeof nonExistent` đang đề cập đến một biến chưa được khai báo `nonExistent`. Thông thường, việc truy cập một tham chiếu biến chưa được khai báo sẽ gây ra ngoại lệ, nhưng toán tử `typeof` được dành cho khả năng đặc biệt để truy cập an toàn ngay cả các định danh không tồn tại và bình tĩnh trả về `"undefined"` thay vì ném ra ngoại lệ. |

Tuy nhiên, mỗi kiểu "rỗng" tương ứng có chính xác một giá trị, cùng tên. Vì vậy `null` là giá trị duy nhất trong kiểu giá trị `null`, và `undefined` là giá trị duy nhất trong kiểu giá trị `undefined`.

### Null'ish

Về mặt ngữ nghĩa, các kiểu `null` và `undefined` đều đại diện cho sự trống rỗng chung, hoặc sự vắng mặt của một giá trị khẳng định, có ý nghĩa khác.

| LƯU Ý: |
| :--- |
| Các hoạt động JS cư xử giống nhau cho dù gặp `null` hay `undefined`, được gọi là "null'ish" (hoặc "nullish"). Tôi đoán "undefined'ish" sẽ trông/nghe quá kỳ quặc! |

Đối với rất nhiều JS, đặc biệt là mã mà các nhà phát triển viết, hai giá trị *nullish* này có thể thay thế cho nhau; quyết định cố ý sử dụng/gán `null` hoặc `undefined` trong bất kỳ tình huống cụ thể nào phụ thuộc vào tình huống và để lại cho nhà phát triển.

JS cung cấp một số khả năng để giúp coi hai giá trị nullish là không thể phân biệt.

Ví dụ, toán tử `==` (so sánh bằng ép buộc) xử lý cụ thể `null` và `undefined` là bằng nhau về mặt ép buộc với nhau, nhưng không bằng với bất kỳ giá trị nào khác trong ngôn ngữ. Do đó, một kiểm tra `.. == null` là an toàn để thực hiện nếu bạn muốn kiểm tra xem một giá trị có cụ thể là `null` hoặc `undefined` hay không:

```js
if (greeting == null) {
    // greeting bị nullish/rỗng
}
```

Một bổ sung khác (gần đây) cho JS là toán tử `??` (nullish-coalescing):

```js
who = myName ?? "User";

// tương đương với:
who = (myName != null) ? myName : "User";
```

Như tương đương ba ngôi minh họa, `??` kiểm tra xem `myName` có phải là non-nullish không, và nếu có, trả về giá trị của nó. Ngược lại, nó trả về toán hạng khác (ở đây là `"User"`).

Cùng với `??`, JS cũng đã thêm toán tử `?.` (chuỗi điều kiện nullish):

```js
record = {
    shippingAddress: {
        street: "123 JS Lane",
        city: "Browserville",
        state: "XY"
    }
};

console.log( record?.shippingAddress?.street );
// 123 JS Lane

console.log( record?.billingAddress?.street );
// undefined
```

Toán tử `?.` kiểm tra giá trị ngay trước (bên trái), và nếu nó là nullish, toán tử dừng lại và trả về giá trị `undefined`. Ngược lại, nó thực hiện truy cập thuộc tính `.` đối với giá trị đó và tiếp tục với biểu thức.

Chỉ để rõ ràng: `record?.` đang nói, "kiểm tra `record` cho nullish trước khi truy cập thuộc tính `.`". Ngoài ra, `billingAddress?.` đang nói, "kiểm tra `billingAddress` cho nullish trước khi truy cập thuộc tính `.`".

| CẢNH BÁO: |
| :--- |
| Một số nhà phát triển JS tin rằng `?.` mới hơn là vượt trội hơn `.`, và do đó hầu như luôn luôn nên được sử dụng thay vì `.`. Tôi tin rằng đó là một quan điểm không khôn ngoan. Trước hết, nó thêm sự lộn xộn trực quan, điều này chỉ nên được thực hiện nếu bạn nhận được lợi ích từ nó. Thứ hai, bạn nên nhận thức được, và lập kế hoạch cho, sự trống rỗng của một số giá trị, để biện minh cho việc sử dụng `?.`. Nếu bạn luôn mong đợi một giá trị non-nullish có mặt trong một biểu thức nào đó, việc sử dụng `?.` để truy cập một thuộc tính trên nó không chỉ không cần thiết/lãng phí, mà còn có khả năng che giấu các lỗi trong tương lai khi giả định về sự hiện diện giá trị của bạn đã thất bại nhưng `?.` đã che đậy nó. Như với hầu hết các tính năng trong JS, hãy sử dụng `.` nơi nó thích hợp nhất, và sử dụng `?.` nơi nó thích hợp nhất. Đừng bao giờ thay thế cái này khi cái kia thích hợp hơn. |

Cũng có một dạng hơi lạ `?.[` của toán tử, không phải `?[`, cho khi bạn cần sử dụng kiểu truy cập `[ .. ]` thay vì truy cập `.`:

```js
record?.["shipping" + "Address"]?.state;    // XY
```

Lại một biến thể khác, được gọi là "optional-call", là `?.(`, và được sử dụng khi gọi một hàm có điều kiện nếu giá trị là non-nullish:

```js
// thay vì:
//   if (someFunc) someFunc(42);
//
// hoặc:
//   someFunc && someFunc(42);

someFunc?.(42);
```

Toán tử `?.(` có vẻ như nó đang kiểm tra xem `someFunc(..)` có phải là một hàm hợp lệ có thể được gọi hay không. Nhưng không phải vậy! Nó chỉ kiểm tra để đảm bảo giá trị là non-nullish trước khi cố gắng gọi nó. Nếu đó là một kiểu giá trị non-nullish khác nhưng cũng không phải là hàm, nỗ lực thực thi vẫn sẽ thất bại với một ngoại lệ `TypeError`.

| CẢNH BÁO: |
| :--- |
| Vì cái bẫy đó, tôi *cực kỳ không thích* dạng toán tử này, và cảnh báo bất kỳ ai không bao giờ sử dụng nó. Tôi nghĩ đó là một tính năng được quan niệm kém gây hại nhiều hơn (cho chính JS, và cho các chương trình) là tốt. Có rất ít tính năng JS mà tôi sẽ đi xa đến mức nói, "đừng bao giờ sử dụng nó." Nhưng đây là một trong những *phần tồi tệ* thực sự của ngôn ngữ, theo ý kiến của tôi. |

### Khác biệt một chút (Distinct'ish)

Điều quan trọng cần ghi nhớ là các kiểu `null` và `undefined` thực sự *là* các kiểu riêng biệt, và do đó `null` có thể khác biệt đáng kể so với `undefined`. Bạn có thể, một cách cẩn thận, xây dựng các chương trình mà hầu như coi chúng là không thể phân biệt. Nhưng điều đó đòi hỏi sự cẩn thận và kỷ luật của nhà phát triển. Từ quan điểm của JS, chúng thường khác biệt hơn.

Có những trường hợp `null` và `undefined` sẽ kích hoạt hành vi khác nhau bởi ngôn ngữ, điều quan trọng cần ghi nhớ. Chúng tôi sẽ không đề cập đến tất cả các trường hợp một cách thấu đáo ở đây, nhưng đây là một ví dụ:

```js
function greet(msg = "Hello") {
    console.log(msg);
}

greet();            // Hello
greet(undefined);   // Hello
greet("Hi");        // Hi

greet(null);        // null
```

Mệnh đề `= ..` trên một tham số được gọi là "mặc định tham số". Nó chỉ kích hoạt và gán giá trị mặc định của nó cho tham số nếu đối số ở vị trí đó bị thiếu, hoặc chính xác là giá trị `undefined`. Nếu bạn truyền `null`, mệnh đề đó không kích hoạt, và `null` do đó được gán cho tham số.

Không có cách *đúng* hay *sai* để sử dụng `null` hoặc `undefined` trong một chương trình. Vì vậy, bài học là: hãy cẩn thận khi chọn giá trị này hay giá trị kia. Và nếu bạn đang sử dụng chúng thay thế cho nhau, hãy cẩn thận hơn nữa.

## Các Giá Trị Boolean

Kiểu `boolean` chứa hai giá trị: `false` và `true`.

Trong "ngày xưa", các ngôn ngữ lập trình, theo quy ước, sẽ sử dụng `0` để có nghĩa là `false` và `1` để có nghĩa là `true`. Vì vậy, bạn có thể nghĩ về kiểu `boolean`, và các từ khóa `false` và `true`, như một cú pháp tiện lợi về mặt ngữ nghĩa trên các giá trị `0` và `1`:

```js
// isLoggedIn = 1;
isLoggedIn = true;

isComplete = 0;
// isComplete = false;
```

Các giá trị Boolean là cách tất cả việc ra quyết định diễn ra trong một chương trình JS:

```js
if (isLoggedIn) {
    // làm gì đó
}

while (!isComplete) {
    // tiếp tục
}
```

Toán tử `!` phủ định/lật một giá trị boolean sang giá trị kia: `false` trở thành `true`, và `true` trở thành `false`.

## Các Giá Trị Chuỗi

Kiểu `string` chứa bất kỳ giá trị nào là tập hợp của một hoặc nhiều ký tự, được phân cách (bao quanh ở hai bên) bởi các ký tự trích dẫn:

```js
myName = "Kyle";
```

JS không phân biệt một ký tự đơn lẻ là một kiểu khác như một số ngôn ngữ làm; `"a"` là một chuỗi giống như `"abc"`.

Các chuỗi có thể được phân cách bằng dấu ngoặc kép (`"`), dấu ngoặc đơn (`'`), hoặc dấu back-tick (`` ` ``). Dấu phân cách kết thúc phải luôn khớp với dấu phân cách bắt đầu.

Các chuỗi có độ dài nội tại tương ứng với bao nhiêu code-point -- thực ra là các đơn vị mã (code-units), sẽ nói thêm về điều đó một chút nữa -- mà chúng chứa.

```js
myName = "Kyle";

myName.length;      // 4
```

Điều này không nhất thiết tương ứng với số lượng ký tự hiển thị hiện diện giữa các dấu phân cách bắt đầu và kết thúc (còn gọi là ký tự chuỗi). Đôi khi có thể hơi khó hiểu để giữ thẳng sự khác biệt giữa một ký tự chuỗi và giá trị chuỗi bên dưới, vì vậy hãy chú ý kỹ.

| LƯU Ý: |
| :--- |
| Chúng ta sẽ đề cập đến việc tính toán độ dài của các chuỗi một cách chi tiết, trong Chương 2. |

### Mã Hóa Ký Tự JS

JS sử dụng loại mã hóa ký tự nào cho các ký tự chuỗi?

Bạn có thể đã nghe nói về "Unicode" và có lẽ ngay cả "UTF-8" (8-bit) hoặc "UTF-16" (16-bit). Nếu bạn giống tôi (trước khi thực hiện nghiên cứu để viết văn bản này), bạn có thể chỉ vẫy tay và quyết định đó là tất cả những gì bạn cần biết về mã hóa ký tự trong các chuỗi JS.

Nhưng... không phải vậy. Thậm chí không gần.

Hóa ra, bạn cần hiểu cách một loạt các khía cạnh của Unicode hoạt động, và thậm chí xem xét các khái niệm từ UCS-2 (Bộ Ký Tự Phổ Quát 2-byte), tương tự như UTF-16, nhưng không hoàn toàn giống. [^UTFUCS]

Unicode định nghĩa tất cả các "ký tự" chúng ta có thể đại diện phổ biến trong các chương trình máy tính, bằng cách gán một số cụ thể cho mỗi ký tự, được gọi là các điểm mã (code-points). Những con số này nằm trong khoảng từ `0` đến tối đa `1114111` (`10FFFF` trong hệ thập lục phân).

Ký hiệu tiêu chuẩn cho các ký tự Unicode là `U+` theo sau là 4-6 ký tự thập lục phân. Ví dụ, `❤` (biểu tượng trái tim) là code-point `10084` (`2764` trong hệ thập lục phân), và do đó được ký hiệu bằng `U+2764`.

Nhóm 65.535 code-point đầu tiên trong Unicode được gọi là BMP (Basic Multilingual Plane - Mặt Phẳng Đa Ngôn Ngữ Cơ Bản). Tất cả những thứ này có thể được biểu diễn bằng 16 bit (2 byte). Khi biểu diễn các ký tự Unicode từ BMP, nó khá đơn giản, vì chúng có thể *vừa vặn* gọn gàng vào các ký tự JS UTF-16 đơn lẻ.

Tất cả các code-point còn lại được nhóm thành 16 cái gọi là "mặt phẳng bổ sung" hoặc "mặt phẳng thiên văn" (astral planes). Các code-point này yêu cầu nhiều hơn 16 bit để biểu diễn -- chính xác là 21 bit -- vì vậy khi biểu diễn các ký tự mở rộng/bổ sung phía trên BMP, JS thực sự lưu trữ các code-point này dưới dạng một cặp hai đơn vị mã 16-bit liền kề, được gọi là *một nửa thay thế* (surrogate halves) (hoặc *cặp thay thế* - surrogate pairs).

Ví dụ, code-point Unicode `127878` (thập lục phân `1F386`) là `🎆` (biểu tượng pháo hoa). JS lưu trữ cái này trong một giá trị chuỗi dưới dạng hai đơn vị mã nửa thay thế: `U+D83C` và `U+DF86`. Hãy nhớ rằng hai phần này của toàn bộ ký tự *không* đứng một mình; chúng chỉ hợp lệ/có ý nghĩa khi được ghép nối ngay lập tức liền kề với nhau.

Điều này có ý nghĩa về độ dài của chuỗi, bởi vì một ký tự hiển thị đơn lẻ như biểu tượng pháo hoa `🎆`, khi ở trong một chuỗi JS, được tính là 2 ký tự cho mục đích của độ dài chuỗi!

Chúng ta sẽ xem xét lại các ký tự Unicode một chút nữa, và sau đó đề cập đến những thách thức của việc tính toán độ dài chuỗi trong Chương 2.

### Các Chuỗi Thoát (Escape Sequences)

Nếu `"` hoặc `'` được sử dụng để phân cách một ký tự chuỗi, nội dung chỉ được phân tích cú pháp cho *các chuỗi thoát ký tự*: `\` theo sau là một hoặc nhiều ký tự mà JS nhận ra và phân tích cú pháp với ý nghĩa đặc biệt. Bất kỳ ký tự nào khác trong một chuỗi không phân tích cú pháp dưới dạng chuỗi thoát (ký tự đơn hoặc đa ký tự), đều được chèn nguyên trạng vào giá trị chuỗi.

Đối với các chuỗi thoát ký tự đơn, các ký tự sau đây được nhận ra sau một `\`: `b`, `f`, `n`, `r`, `t`, `v`, `0`, `'`, `"`, và `\`. Ví dụ, `\n` có nghĩa là dòng mới, `\t` có nghĩa là tab, v.v.

Nếu một `\` được theo sau bởi bất kỳ ký tự nào khác (ngoại trừ `x` và `u` -- được giải thích bên dưới), ví dụ như `\k`, chuỗi đó được hiểu là `\` là một sự thoát không cần thiết, do đó bị loại bỏ, chỉ để lại ký tự chữ đen (literal character) đó (`k`).

Để bao gồm một `"` ở giữa một ký tự chuỗi được phân cách bởi `"`, hãy sử dụng chuỗi thoát `\"`. Tương tự, nếu bạn bao gồm một ký tự `'` ở giữa một ký tự chuỗi được phân cách bởi `'`, hãy sử dụng chuỗi thoát `\'`. Ngược lại, một `'` *không* cần phải được thoát bên trong một chuỗi được phân cách bởi `"`, cũng như ngược lại.

```js
myTitle = "Kyle Simpson (aka, \"getify\"), former O'Reilly author";

console.log(myTitle);
// Kyle Simpson (aka, "getify"), former O'Reilly author
```

Trong văn bản, dấu gạch chéo `/` là phổ biến nhất. Nhưng thỉnh thoảng, bạn cần một dấu gạch chéo ngược `\`. Để bao gồm một ký tự gạch chéo ngược `\` theo nghĩa đen mà không thực hiện như sự bắt đầu của một chuỗi thoát ký tự, hãy sử dụng `\\` (hai dấu gạch chéo ngược).

Vậy thì... `\\\` (ba dấu gạch chéo ngược) trong một chuỗi sẽ phân tích cú pháp như thế nào? Hai `\` đầu tiên sẽ là một chuỗi thoát `\\`, do đó chèn chỉ một ký tự `\` duy nhất trong giá trị chuỗi, và `\` còn lại sẽ chỉ thoát bất kỳ ký tự nào đến ngay sau nó.

Một nơi các dấu gạch chéo ngược xuất hiện phổ biến là trong các đường dẫn tệp Windows, sử dụng dấu phân cách `\` thay vì dấu phân cách `/` được sử dụng trong các đường dẫn kiểu linux/unix:

```js
windowsFontsPath =
    "C:\\Windows\\Fonts\\";

console.log(windowsFontsPath);
// C:\Windows\Fonts\"
```

| MẸO: |
| :--- |
| Còn bốn dấu gạch chéo ngược `\\\\` trong một ký tự chuỗi thì sao? Chà, đó chỉ là hai chuỗi thoát `\\` cạnh nhau, vì vậy nó dẫn đến hai dấu gạch chéo ngược liền kề (`\\`) trong giá trị chuỗi bên dưới. Bạn có thể nhận ra có một mô hình quy tắc lẻ/chẵn đang diễn ra. Do đó, bạn sẽ có thể giải mã bất kỳ số lẻ (`\\\\\`, `\\\\\\\\\`, v.v.) hoặc chẵn (`\\\\\\`, `\\\\\\\\\\`, v.v.) của các dấu gạch chéo ngược trong một ký tự chuỗi. |

#### Tiếp Tục Dòng

Ký tự `\` theo sau là một ký tự dòng mới thực tế (không chỉ là `n` theo nghĩa đen) là một trường hợp đặc biệt, và nó tạo ra những gì được gọi là tiếp tục dòng (line-continuation):

```js
greeting = "Hello \
Friends!";

console.log(greeting);
// Hello Friends!
```

Như bạn có thể thấy, dòng mới ở cuối dòng `greeting =` ngay lập tức được đi trước bởi một `\`, cho phép ký tự chuỗi này tiếp tục vào dòng tiếp theo. Nếu không có dấu `\` thoát trước nó, một dòng mới -- dòng mới thực tế, không phải chuỗi thoát ký tự `\n` -- xuất hiện trong một ký tự chuỗi được phân cách bởi `"` hoặc `'` thực sự sẽ tạo ra một lỗi phân tích cú pháp cú pháp JS.

Bởi vì `\` cuối dòng biến ký tự dòng mới thành một sự tiếp tục dòng, ký tự dòng mới bị bỏ qua khỏi chuỗi, như được hiển thị bởi đầu ra `console.log(..)`.

| LƯU Ý: |
| :--- |
| Tính năng tiếp tục dòng này thường được gọi là "chuỗi nhiều dòng", nhưng tôi nghĩ đó là một nhãn gây nhầm lẫn. Như bạn có thể thấy, bản thân giá trị chuỗi không có nhiều dòng, nó chỉ được định nghĩa qua nhiều dòng thông qua các tiếp tục dòng. Một chuỗi nhiều dòng thực sự sẽ có nhiều dòng trong giá trị bên dưới. Chúng ta sẽ xem xét lại chủ đề này sau trong chương này khi chúng ta đề cập đến Template Literals. |

### Các Chuỗi Thoát Đa Ký Tự

Các chuỗi thoát đa ký tự có thể là các chuỗi thập lục phân hoặc Unicode.

Các chuỗi thoát thập lục phân được sử dụng để mã hóa bất kỳ ký tự ASCII cơ sở nào (mã 0-255), và trông giống như `\x` theo sau bởi chính xác hai ký tự thập lục phân (`0-9` và `a-f` / `A-F` -- không phân biệt chữ hoa thường). Ví dụ, `A9` hoặc `a9` là giá trị thập phân `169`, tương ứng với:

```js
copyright = "\xA9";  // or "\xa9"

console.log(copyright);     // ©
```

Đối với bất kỳ ký tự bình thường nào có thể được gõ trên bàn phím, chẳng hạn như `"a"`, thường dễ đọc nhất là chỉ định ký tự chữ đen, thay vì một biểu diễn thập lục phân khó hiểu hơn:

```js
"a" === "\x61";             // true
```

#### Unicode Trong Chuỗi

Các chuỗi thoát Unicode đơn lẻ có thể mã hóa bất kỳ ký tự nào từ Unicode BMP. Chúng trông giống như `\u` theo sau bởi chính xác bốn ký tự thập lục phân.

Ví dụ, chuỗi thoát `\u00A9` (hoặc `\u00a9`) tương ứng với cùng biểu tượng `©` đó, trong khi `\u263A` (hoặc `\u263a`) tương ứng với ký tự Unicode có code-point `9786`: `☺` (biểu tượng mặt cười).

Khi bất kỳ chuỗi thoát ký tự nào (bất kể độ dài) được nhận ra, ký tự đơn lẻ mà nó đại diện được chèn vào chuỗi, thay vì các ký tự riêng biệt ban đầu. Vì vậy, trong chuỗi `"\u263A"`, chỉ có một ký tự (mặt cười), không phải sáu ký tự riêng lẻ.

Nhưng như đã giải thích trước đó, nhiều code-point Unicode nằm cao hơn `65535`. Ví dụ, `1F4A9` (hoặc `1f4a9`) là code-point thập phân `128169`, tương ứng với biểu tượng vui nhộn `💩` (đống phân).

Nhưng `\u1F4A9` sẽ không hoạt động để bao gồm ký tự này trong một chuỗi, vì nó sẽ được phân tích cú pháp là chuỗi thoát Unicode `\u1F4A`, theo sau là một ký tự `9` theo nghĩa đen. Để giải quyết hạn chế này, một biến thể của các chuỗi thoát Unicode đã được giới thiệu để cho phép một số lượng ký tự thập lục phân tùy ý sau `\u`, bằng cách bao quanh chúng bằng dấu ngoặc nhọn `{ .. }`:

```js
myReaction = "\u{1F4A9}";

console.log(myReaction);
// 💩
```

Hãy nhớ lại cuộc thảo luận trước đó về các ký tự Unicode mở rộng (không phải BMP) và *các nửa thay thế* (surrogate halves)? Cùng một `💩` đó cũng có thể được định nghĩa với hai đơn vị mã rõ ràng, tạo thành một cặp thay thế:

```js
myReaction = "\uD83D\uDCA9";

console.log(myReaction);
// 💩
```

Cả ba biểu diễn của cùng ký tự này được lưu trữ nội bộ bởi JS giống hệt nhau, và không thể phân biệt:

```js
"💩" === "\u{1F4A9}";                // true
"\u{1F4A9}" === "\uD83D\uDCA9";     // true
```

Mặc dù JS không quan tâm cách biểu diễn ký tự như vậy trong chương trình của bạn, hãy xem xét cẩn thận sự khác biệt về khả năng đọc khi soạn thảo mã của bạn.

| LƯU Ý: |
| :--- |
| Mặc dù `💩` trông giống như một ký tự đơn lẻ, biểu diễn nội bộ của nó ảnh hưởng đến những thứ như tính toán độ dài của một chuỗi có ký tự đó trong đó. Chúng ta sẽ đề cập đến việc tính toán độ dài của chuỗi trong Chương 2. |

##### Chuẩn Hóa Unicode

Một nếp nhăn khác trong xử lý chuỗi Unicode là ngay cả một số ký tự BMP đơn lẻ nhất định cũng có thể được biểu diễn theo những cách khác nhau.

Ví dụ, ký tự `"é"` có thể được biểu diễn dưới dạng chính nó (code-point `233`, hay còn gọi là `\xe9` hoặc `\u00e9` hoặc `\u{e9}`), hoặc dưới dạng sự kết hợp của hai code-point: ký tự `"e"` (code-point `101`, hay còn gọi là `\x65`, `\u0065`, `\u{65}`) và *dấu ngã kết hợp* (code-point `769`, hay còn gọi là `\u0301`, `\u{301}`).

Hãy xem xét:

```js
eTilde1 = "é";
eTilde2 = "\u00e9";
eTilde3 = "\u0065\u0301";

console.log(eTilde1);       // é
console.log(eTilde2);       // é
console.log(eTilde3);       // é
```

Ký tự chuỗi được gán cho `eTilde3` trong đoạn trích này lưu trữ dấu trọng âm dưới dạng một biểu tượng *dấu kết hợp* riêng biệt. Giống như các cặp thay thế, một dấu kết hợp chỉ có ý nghĩa liên quan đến biểu tượng mà nó liền kề (thường là sau).

Việc hiển thị biểu tượng Unicode phải giống nhau bất kể, nhưng cách ký tự `"é"` được lưu trữ nội bộ ảnh hưởng đến những thứ như tính toán `length` của chuỗi chứa, cũng như so sánh bằng và quan hệ (thêm về những điều này trong Chương 2):

```js
eTilde1.length;             // 2
eTilde2.length;             // 1
eTilde3.length;             // 2

eTilde1 === eTilde2;        // false
eTilde1 === eTilde3;        // true
```

Một thách thức cụ thể là bạn có thể sao chép-dán một chuỗi với một ký tự `"é"` hiển thị trong đó, và ký tự bạn đã sao chép có thể ở dạng *được kết hợp* hoặc *được phân tách*. Nhưng không có cách trực quan nào để biết, và tuy nhiên giá trị chuỗi bên dưới trong ký tự chuỗi sẽ khác nhau:

```js
"é" === "é";           // false!!
```

Sự khác biệt biểu diễn nội bộ này có thể khá thách thức nếu không được lập kế hoạch cẩn thận. May mắn thay, JS cung cấp một phương thức tiện ích `normalize(..)` trên các chuỗi để giúp đỡ:

```js
eTilde1 = "é";
eTilde2 = "\u{e9}";
eTilde3 = "\u{65}\u{301}";

eTilde1.normalize("NFC") === eTilde2;
eTilde2.normalize("NFD") === eTilde3;
```

Chế độ chuẩn hóa `"NFC"` kết hợp các code-point liền kề thành code-point *được kết hợp* (nếu có thể), trong khi chế độ chuẩn hóa `"NFD"` phân tách một code-point đơn lẻ thành các code-point *được phân tách* của nó (nếu có thể).

Và thực sự có thể có nhiều hơn hai code-point *được phân tách* riêng lẻ tạo nên một code-point *được kết hợp* đơn lẻ -- ví dụ, một ký tự đơn lẻ có thể có một vài dấu phụ được áp dụng cho nó.

Khi xử lý các chuỗi Unicode sẽ được so sánh, sắp xếp, hoặc phân tích độ dài, điều rất quan trọng cần ghi nhớ là chuẩn hóa Unicode, và sử dụng nó khi cần thiết.

##### Các Cụm Hình Vị Unicode

Một biến chứng cuối cùng của việc xử lý chuỗi Unicode là hỗ trợ cho việc phân cụm nhiều code-point liền kề thành một biểu tượng phân biệt trực quan duy nhất, được gọi là một *hình vị* (hoặc một *cụm hình vị*).

Một ví dụ sẽ là một emoji gia đình như `"👩‍👩‍👦‍👦"`, thực sự được tạo thành từ 7 code-point tất cả cụm/nhóm lại với nhau thành một biểu tượng trực quan duy nhất.

Hãy xem xét:

```js
familyEmoji = "\u{1f469}\u{200d}\u{1f469}\u{200d}\u{1f466}\u{200d}\u{1f466}";

familyEmoji;            // 👩‍👩‍👦‍👦
```

Emoji này *không* phải là một code-point Unicode đã đăng ký đơn lẻ, và như vậy, không có *sự chuẩn hóa* nào có thể được thực hiện để kết hợp 7 code-point riêng biệt này thành một thực thể duy nhất. Logic hiển thị trực quan cho các biểu tượng tổng hợp như vậy khá phức tạp, vượt xa những gì hầu hết các nhà phát triển JS muốn nhúng vào chương trình của chúng ta. Các thư viện tồn tại để xử lý một số logic này, nhưng chũng thường lớn và vẫn không nhất thiết bao gồm tất cả các sắc thái/biến thể.

Không giống như các cặp thay thế và các dấu kết hợp, các biểu tượng trong các cụm hình vị thực sự có thể hoạt động như các ký tự độc lập, nhưng có hành vi kết hợp đặc biệt khi được đặt liền kề với nhau.

Loại phức tạp này ảnh hưởng đáng kể đến các tính toán độ dài, so sánh, sắp xếp, và nhiều hoạt động hướng chuỗi phổ biến khác.

### Template Literals

Tôi đã đề cập trước đó rằng các chuỗi có thể luân phiên được phân cách bằng dấu back-tick `` `..` ``:

```js
myName = `Kyle`;
```

Tất cả các quy tắc tương tự cho mã hóa ký tự, chuỗi thoát ký tự, và độ dài đều áp dụng cho các loại chuỗi này.

Tuy nhiên, nội dung của các template (string) literals này được phân tích cú pháp bổ sung cho một chuỗi phân cách đặc biệt `${ .. }`, đánh dấu một biểu thức để đánh giá và nội suy vào giá trị chuỗi tại vị trí đó:

```js
myName = `Kyle`;

greeting = `Hello, ${myName}!`;

console.log(greeting);      // Hello, Kyle!
```

Mọi thứ giữa `{ .. }` trong một template literal như vậy là một biểu thức JS tùy ý. Nó có thể là các biến đơn giản như `myName`, hoặc các chương trình JS phức tạp, hoặc bất cứ thứ gì ở giữa (thậm chí một biểu thức template literal khác!).

| MẸO: |
| :--- |
| Tính năng này thường được gọi là "template literals" hoặc "template strings", nhưng tôi nghĩ điều đó gây nhầm lẫn. "Template" thường có nghĩa là, trong bối cảnh lập trình, một tập hợp văn bản có thể tái sử dụng có thể được đánh giá lại với dữ liệu khác nhau. Ví dụ, *công cụ mẫu* (template engines) cho các trang, mẫu email cho các chiến dịch bản tin, v.v. Tính năng JS này không có khả năng tái sử dụng. Nó là một ký tự literal, và nó tạo ra một giá trị đơn lẻ, tức thì (thường là một chuỗi). Bạn có thể đặt một giá trị như vậy trong một hàm, và gọi hàm nhiều lần. Nhưng sau đó hàm đang hoạt động như mẫu, không phải bản thân literal. Tôi thích thay vào đó gọi tính năng này là *interpolated literals*, hoặc cách viết tắt vui nhộn: *interpoliterals*. Tôi chỉ nghĩ cái tên đó mô tả chính xác hơn. |

Template literals cũng có một hành vi khác thú vị liên quan đến các dòng mới, so với các chuỗi được phân cách bằng `"` hoặc `'` cổ điển. Hãy nhớ lại rằng đối với các chuỗi đó, một sự tiếp tục dòng yêu cầu một `\` ở cuối mỗi dòng, ngay trước một dòng mới. Không phải vậy, với template literals!

```js
myPoem = `
Roses are red
Violets are blue
C3PO's a funny robot
and so R2.`;

console.log(myPoem);
//
// Roses are red
// Violets are blue
// C3PO's a funny robot
// and so R2.
```

Các sự tiếp tục dòng với template literals *không yêu cầu* thoát. Tuy nhiên, điều đó có nghĩa là dòng mới là một phần của chuỗi, ngay cả dòng mới đầu tiên ở trên. Nói cách khác, `myPoem` ở trên giữ một *chuỗi nhiều dòng* thực sự, như được hiển thị. Tuy nhiên, nếu bạn thoát `\` ở cuối bất kỳ dòng nào trong một template literal, dòng mới sẽ bị bỏ qua, giống như với các chuỗi không phải template literal.

Template literals thường dẫn đến một giá trị chuỗi, nhưng không phải luôn luôn. Một dạng của template literal có thể trông hơi lạ được gọi là *tagged template literal*:

```js
price = formatCurrency`The cost is: ${totalCost}`;
```

Ở đây, `formatCurrency` là một thẻ được áp dụng cho giá trị template literal, thực sự gọi `formatCurrency(..)` như một hàm, truyền cho nó các ký tự chuỗi và các biểu thức nội suy được phân tích cú pháp từ giá trị. Hàm này sau đó có thể lắp ráp chúng theo bất kỳ cách nào nó thấy phù hợp -- chẳng hạn như định dạng một giá trị `number` thành tiền tệ trong locale hiện tại -- và trả về bất kỳ giá trị nào, chuỗi hoặc cái gì khác, mà nó muốn.

Vì vậy các tagged template literals không phải luôn là chuỗi; chúng có thể là bất kỳ giá trị nào. Nhưng các template literals không được gắn thẻ *sẽ luôn là* chuỗi.

Một số nhà phát triển JS tin rằng các chuỗi template literal không được gắn thẻ là tốt nhất để sử dụng cho *tất cả* các chuỗi, ngay cả khi không sử dụng bất kỳ nội suy biểu thức hoặc nhiều dòng nào. Tôi không đồng ý. Tôi nghĩ chúng chỉ nên được sử dụng khi nội suy (hoặc nhiều dòng).

| MẸO: |
| :--- |
| Nguyên tắc tôi luôn áp dụng trong việc đưa ra các quyết định như vậy: sử dụng tính năng/công cụ khớp gần nhất, và ít khả năng nhất, cho bất kỳ nhiệm vụ nào. |

Hơn nữa, có một vài nơi mà các chuỗi kiểu `` `..` `` bị cấm. Ví dụ, pragma `"use strict"` không thể sử dụng back-ticks, hoặc pragma sẽ bị bỏ qua một cách âm thầm (và do đó chương trình vô tình chạy trong chế độ không nghiêm ngặt). Ngoài ra, kiểu chuỗi này không thể được sử dụng trong các tên thuộc tính được trích dẫn của các object literals, các mẫu destructuring, hoặc trong mệnh đề chỉ định mô-đun `import .. from ..` của ES Module.

Quan điểm của tôi: sử dụng các chuỗi được phân cách bởi `` `..` `` ở nơi được phép, nhưng chỉ khi cần nội suy/nhiều dòng; và tiếp tục sử dụng các chuỗi được phân cách bởi `".."` hoặc `'..'` cho mọi thứ khác.

## Các Giá Trị Số

Kiểu `number` chứa bất kỳ giá trị số nào (số nguyên hoặc thập phân), chẳng hạn như `-42` hoặc `3.1415926`. Các giá trị này được biểu diễn bởi công cụ JS dưới dạng các giá trị dấu phẩy động nhị phân độ chính xác kép 64-bit, IEEE-754. [^IEEE754]

Các `number` JS luôn là số thập phân; các số nguyên (còn gọi là "integers") không được lưu trữ theo cách khác biệt/đặc biệt. Một "số nguyên" được lưu trữ dưới dạng một giá trị `number` chỉ đơn thuần là không có gì khác không làm phần phân số của nó; `42` do đó không thể phân biệt trong JS với `42.0` và `42.000000`.

Chúng ta có thể sử dụng `Number.isInteger(..)` để xác định xem một giá trị `number` có bất kỳ phân số khác không nào hay không:

```js
Number.isInteger(42);           // true
Number.isInteger(42.0);         // true
Number.isInteger(42.000000);    // true

Number.isInteger(42.0000001);   // false
```

### Phân Tích Cú Pháp vs Ép Buộc

Nếu một giá trị chuỗi giữ nội dung trông giống số, bạn có thể cần chuyển đổi từ giá trị chuỗi đó sang một `number`, cho các mục đích toán học.

Tuy nhiên, điều rất quan trọng là phân biệt giữa chuyển đổi phân tích cú pháp (parsing-conversion) và chuyển đổi ép buộc (coercive-conversion).

Chúng ta có thể chuyển đổi phân tích cú pháp với các tiện ích `parseInt(..)` hoặc `parseFloat(..)` được tích hợp sẵn của JS:

```js
someNumericText = "123.456";

parseInt(someNumericText,10);               // 123
parseFloat(someNumericText);                // 123.456

parseInt("42",10) === parseFloat("42");     // true

parseInt("512px");                          // 512
```

| LƯU Ý: |
| :--- |
| Phân tích cú pháp chỉ có liên quan đối với các giá trị chuỗi, vì nó là một hoạt động từng ký tự (trái sang phải). Sẽ không có ý nghĩa gì khi phân tích cú pháp nội dung của một `boolean`, cũng như phân tích cú pháp nội dung của một `number` hoặc một `null`; không có gì để phân tích cú pháp. Nếu bạn truyền bất cứ thứ gì khác ngoài một giá trị chuỗi cho `parseInt(..)` / `parseFloat(..)`, các tiện ích đó trước tiên chuyển đổi giá trị đó thành một chuỗi và sau đó cố gắng phân tích cú pháp nó. Điều đó gần như chắc chắn có vấn đề (dẫn đến lỗi) hoặc lãng phí -- `parseInt(42)` là ngớ ngẩn, và `parseInt(42.3)` là một sự lạm dụng `parseInt(..)` để thực hiện công việc của `Math.floor(..)`. |

Phân tích cú pháp lấy ra các ký tự trông giống số từ giá trị chuỗi, và đưa chúng vào một giá trị `number`, dừng lại ngay khi nó gặp một ký tự không phải số (ví dụ: không phải `-`, `.` hoặc `0`-`9`). Nếu phân tích cú pháp thất bại ở ký tự đầu tiên, cả hai tiện ích đều trả về giá trị đặc biệt `NaN` (xem "Số Không Hợp Lệ" bên dưới), cho biết hoạt động không hợp lệ và đã thất bại.

Khi `parseInt(..)` gặp `.` trong `"123.456"`, nó dừng lại, chỉ sử dụng `123` trong giá trị `number` kết quả. `parseFloat(..)` ngược lại chấp nhận ký tự `.` này, và tiếp tục phân tích cú pháp một số thập phân với bất kỳ chữ số thập phân nào sau `.`.

Tiện ích `parseInt(..)` cụ thể, nhận một đối số thứ hai tùy chọn -- nhưng *thực tế*, khá cần thiết --, `radix`: cơ sở số để giả định cho việc thông dịch các ký tự chuỗi cho `number` (phạm vi `2` - `36`). `10` là cho các số cơ sở 10 tiêu chuẩn, `2` là cho nhị phân, `8` là cho bát phân, và `16` là cho thập lục phân. Bất kỳ `radix` bất thường nào khác, như `23`, giả định các chữ số theo thứ tự, `0` - `9` theo sau là thứ tự ký tự `a` - `z` (không phân biệt chữ hoa thường). Nếu radix được chỉ định nằm ngoài phạm vi `2` - `36`, `parseInt(..)` thất bại là không hợp lệ và trả về giá trị `NaN`.

Nếu `radix` bị bỏ qua, hành vi của `parseInt(..)` khá tinh tế và khó hiểu, ở chỗ nó cố gắng đoán tốt nhất cho một radix, dựa trên những gì nó thấy trong ký tự đầu tiên. Điều này trong lịch sử đã dẫn đến rất nhiều lỗi tinh vi, vì vậy đừng bao giờ dựa vào việc tự động đoán mặc định; luôn chỉ định một radix rõ ràng (như `10` trong các lệnh gọi ở trên).

`parseFloat(..)` luôn phân tích cú pháp với một radix là `10`, vì vậy không có đối số thứ hai nào được chấp nhận.

| CẢNH BÁO: |
| :--- |
| Một sự khác biệt đáng ngạc nhiên giữa `parseInt(..)` và `parseFloat(..)` là `parseInt(..)` sẽ không phân tích cú pháp đầy đủ ký hiệu khoa học (ví dụ: `"1.23e+5"`), thay vào đó dừng lại ở `.` vì nó không hợp lệ cho số nguyên; trên thực tế, ngay cả `"1e+5"` cũng dừng lại ở `"e"`. `parseFloat(..)` mặt khác phân tích cú pháp đầy đủ ký hiệu khoa học như mong đợi. |

Trái ngược với chuyển đổi phân tích cú pháp, chuyển đổi ép buộc là một loại hoạt động tất cả hoặc không có gì. Hoặc toàn bộ nội dung của chuỗi được nhận ra là số (số nguyên hoặc số thập phân), hoặc toàn bộ chuyển đổi thất bại (dẫn đến `NaN` -- một lần nữa, xem "Số Không Hợp Lệ" sau trong chương này).

Chuyển đổi ép buộc có thể được thực hiện một cách rõ ràng với hàm `Number(..)` (không có từ khóa `new`) hoặc với toán tử một ngôi `+` phía trước giá trị:

```js
someNumericText = "123.456";

Number(someNumericText);        // 123.456
+someNumericText;               // 123.456

Number("512px");                // NaN
+"512px";                       // NaN
```

### Các Biểu Diễn Số Khác

Ngoài việc định nghĩa các số bằng các chữ số cơ sở 10 truyền thống (`0`-`9`), JS hỗ trợ định nghĩa các literal số chỉ toàn số nguyên trong ba cơ sở khác: nhị phân (cơ sở 2), bát phân (cơ sở 8), và thập lục phân (cơ sở 16).

```js
// binary
myAge = 0b101010;
myAge;              // 42

// octal
myAge = 0o52;
myAge;              // 42

// hexadecimal
myAge = 0x2a;
myAge;              // 42
```

Như bạn có thể thấy, các tiền tố `0b` (nhị phân), `0o` (bát phân), và `0x` (thập lục phân) báo hiệu việc xác định các số trong các cơ sở khác nhau, nhưng các số thập phân không được phép trên các literal số này.

| LƯU Ý: |
| :--- |
| Cú pháp JS cho phép các tiền tố `0B`, `0O`, và `0X` cũng như vậy. Tuy nhiên, xin đừng bao giờ sử dụng các dạng tiền tố chữ hoa đó. Tôi nghĩ bất kỳ người nhạy cảm nào cũng sẽ đồng ý: `0O` dễ bị nhầm lẫn hơn nhiều khi nhìn lướt qua so với `0o` (bản thân nó cũng hơi mơ hồ về mặt hình ảnh khi nhìn lướt qua). Luôn gắn bó với các dạng tiền tố chữ thường! |

Điều quan trọng là phải nhận ra rằng bạn không đang định nghĩa một *số khác*, chỉ là sử dụng một dạng khác để tạo ra cùng một giá trị số bên dưới.

Theo mặc định, JS đại diện cho giá trị số bên dưới theo kiểu đầu ra/chuỗi với dạng cơ sở 10 tiêu chuẩn. Tuy nhiên, các giá trị `number` có một phương thức `toString(..)` được tích hợp sẵn tạo ra một biểu diễn chuỗi trong bất kỳ cơ sở/radix được chỉ định nào (như với `parseInt(..)`, trong phạm vi `2` - `36`):

```js
myAge = 42;

myAge.toString(2);          // "101010"
myAge.toString(8);          // "52"
myAge.toString(16);         // "2a"
myAge.toString(23);         // "1j"
myAge.toString(36);         // "16"
```

Bạn có thể khứ hồi bất kỳ biểu diễn chuỗi radix tùy ý nào trở lại thành một `number` bằng cách sử dụng `parseInt(..)`, với radix thích hợp:

```js
myAge = 42;

parseInt(myAge.toString("23"),23);      // 42
```

Một dạng được phép khác để chỉ định các literal số là sử dụng ký hiệu khoa học:

```js
myAge = 4.2E1;      // or 4.2e1 or 4.2e+1

myAge;              // 42
```

`4.2E1` (hoặc `4.2e1`) có nghĩa là, `4.2 * (10 ** 1)` (`10` mũ `1`). Số mũ có thể tùy chọn có dấu `+` hoặc `-`. Nếu dấu bị bỏ qua, nó được cho là `+`. Một số mũ âm làm cho số nhỏ hơn (di chuyển dấu thập phân sang trái) thay vì lớn hơn (di chuyển dấu thập phân sang phải):

```js
4.2E-3;             // 0.0042
```

Dạng ký hiệu khoa học này đặc biệt hữu ích cho khả năng đọc khi chỉ định các lũy thừa lớn hơn của `10`:

```js
someBigPowerOf10 = 1000000000;

// vs:

someBigPowerOf10 = 1e9;
```

Theo mặc định, JS sẽ đại diện (ví dụ, dưới dạng giá trị chuỗi, v.v.) hoặc các số rất lớn hoặc rất nhỏ -- cụ thể, nếu các giá trị yêu cầu nhiều hơn 21 chữ số chính xác -- sử dụng cùng dạng ký hiệu khoa học này:

```js
ratherBigNumber = 123 ** 11;
ratherBigNumber.toString();     // "9.748913698143826e+22"

prettySmallNumber = 123 ** -11;
prettySmallNumber.toString();   // "1.0257553107587752e-23"
```

Các số có giá trị tuyệt đối nhỏ hơn (gần `0` hơn) so với các ngưỡng này vẫn có thể bị buộc vào dạng ký hiệu khoa học (dưới dạng chuỗi):

```js
plainBoringNumber = 42;

plainBoringNumber.toExponential();      // "4.2e+1"
plainBoringNumber.toExponential(0);     // "4e+1"
plainBoringNumber.toExponential(4);     // "4.2000e+1"
```

Đối số tùy chọn cho `toExponential(..)` chỉ định số lượng chữ số thập phân cần bao gồm trong biểu diễn chuỗi.

Một khả năng đọc khác để chỉ định các literal số trong mã là khả năng chèn `_` làm dấu phân cách chữ số bất cứ nơi nào thuận tiện/có ý nghĩa để làm như vậy. Ví dụ:

```js
someBigPowerOf10 = 1_000_000_000;

totalCostInPennies = 123_45;  // vs 12_345
```

Quyết định sử dụng `12345` (không có dấu phân cách), `12_345` (giống như "12,345"), hoặc `123_45` (giống như "123.45") hoàn toàn tùy thuộc vào tác giả của mã; JS bỏ qua các dấu phân cách. Nhưng tùy thuộc vào ngữ cảnh, `123_45` có thể có ý nghĩa về mặt ngữ nghĩa (về mặt khả năng đọc) hơn là kiểu nhóm ba chữ số từ phải sang trái phân cách bằng dấu phẩy truyền thống được bắt chước với `12_345`.

### Biểu Diễn Nhị Phân Bitwise IEEE-754

IEEE-754[^IEEE754] là một tiêu chuẩn kỹ thuật cho biểu diễn nhị phân của các số thập phân. Nó được sử dụng rộng rãi bởi hầu hết các ngôn ngữ lập trình máy tính, bao gồm JS, Python, Ruby, v.v.

Tôi sẽ không đề cập đến nó một cách thấu đáo, nhưng tôi nghĩ một bài giới thiệu ngắn gọn về cách các con số hoạt động trong các ngôn ngữ như JS là hơn mức cần thiết, vì rất ít lập trình viên có *bất kỳ* sự quen thuộc nào với nó.

Trong IEEE-754 64-bit -- được gọi là "độ chính xác kép" (double-precision), bởi vì ban đầu IEEE-754 từng là 32-bit, và bây giờ nó gấp đôi thế! -- 64 bit được chia thành ba phần: 52 bit cho giá trị cơ sở của số (hay còn gọi là "phần phân số", "mantissa", hoặc "significand"), 11 bit cho số mũ để nâng `2` lên trước khi nhân, và 1 bit cho dấu của giá trị cuối cùng.

| LƯU Ý: |
| :--- |
| Vì chỉ có 52 trong số 64 bit thực sự được sử dụng để đại diện cho giá trị cơ sở, `number` không thực sự có `2^64` giá trị trong đó. Theo đặc tả cho kiểu `number`[^NumberType], số lượng giá trị chính xác là `2^64 - 2^53 + 3`, hoặc khoảng 18 tỷ tỷ (quintillion), chia đều giữa các số dương và số âm. |

Các bit này được sắp xếp từ trái sang phải, như sau (S = Bit Dấu, E = Bit Số Mũ, M = Bit Mantissa):

```js
SEEEEEEEEEEEMMMMMMMMMMMMMMMMMMMM
MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM
```

Vì vậy, số `42` (hoặc `42.000000`) sẽ được biểu diễn bởi các bit này:

```text
// 42:
01000000010001010000000000000000
00000000000000000000000000000000
```

Bit dấu là `0`, có nghĩa là số dương (`1` có nghĩa là âm).

Số mũ 11-bit là nhị phân `10000000100`, trong cơ sở 10 là `1028`. Nhưng trong IEEE-754, giá trị này được diễn giải là được lưu trữ không dấu với "độ lệch số mũ" (exponent bias) là `1023`, có nghĩa là chúng ta đang thay đổi phạm vi số mũ từ `-1022:1023` thành `1:2046` (nơi `0` và `2047` được dành riêng cho các biểu diễn đặc biệt). Vì vậy, lấy `1028` trừ đi độ lệch `1023`, cho ra số mũ hiệu quả là `5`. Chúng ta nâng `2` lên giá trị đó (`2^5`), cho ra `32`.

| LƯU Ý: |
| :--- |
| Nếu việc trừ `1023` khỏi giá trị số mũ cho ra một số âm (ví dụ, `-3`), điều đó vẫn được diễn giải là số mũ của `2`; nâng `2` lên các số âm chỉ tạo ra các giá trị nhỏ hơn và nhỏ hơn. |

52 bit còn lại cung cấp cho chúng ta giá trị cơ sở `01010000...`, được diễn giải là số thập phân nhị phân `1.0101000...` (với tất cả các số không theo sau). Chuyển đổi *cái đó* sang cơ sở 10, chúng ta nhận được `1.3125000...`. Cuối cùng, nhân nó với `32` đã được tính toán từ số mũ. Kết quả: `42`.

Như bạn có thể có thể nhận ra bây giờ, tiêu chuẩn biểu diễn số IEEE-754 này được gọi là "dấu phẩy động" (floating point) bởi vì dấu thập phân "trôi nổi" qua lại dọc theo các bit, tùy thuộc vào giá trị số mũ được chỉ định.

Số `42.0000001`, chỉ khác với `42.000000` bởi chỉ `0.0000001`, sẽ được biểu diễn bởi các bit này:

```text
// 42.0000001:
01000000010001010000000000000000
00000000110101101011111110010101
```

Hãy chú ý cách mẫu bit trước đó và mẫu này khác nhau khá nhiều bit ở các vị trí cuối cùng! Phân số thập phân nhị phân chứa tất cả các bit `1` thừa đó (`1.010100000000...01011111110010101`) chuyển đổi sang cơ sở 10 là `1.31250000312500003652`, nhân với `32` cho chúng ta chính xác `42.0000001`.

Chúng ta sẽ xem xét lại nhiều chi tiết hơn về độ (không) chính xác dấu phẩy động trong Chương 2. Nhưng bây giờ bạn đã hiểu thêm một *chút* về cách IEEE-754 hoạt động!

### Các Giới Hạn Số

Như có thể thấy rõ bây giờ khi bạn đã thấy cách IEEE-754 hoạt động, 52 bit của cơ sở số phải được chia sẻ, đại diện cho cả phần nguyên (nếu có) cũng như phần thập phân (nếu có), của giá trị `number` dự định. Về cơ bản, phần nguyên càng lớn được biểu diễn, càng ít bit có sẵn cho phần thập phân, và ngược lại.

Giá trị lớn nhất có thể được lưu trữ chính xác trong kiểu `number` được hiển thị dưới dạng `Number.MAX_VALUE`:

```js
Number.MAX_VALUE;           // 1.7976931348623157e+308
```

Bạn có thể mong đợi giá trị đó là một giá trị thập phân, dựa trên biểu diễn. Nhưng khi kiểm tra kỹ hơn, `1.79E308` (xấp xỉ) là `2^1024 - 1`. Điều đó có vẻ giống như nó nên là một số nguyên hơn, phải không? Chúng ta có thể xác minh:

```js
Number.isInteger(Number.MAX_VALUE);         // true
```

Nhưng điều gì xảy ra nếu bạn vượt quá giá trị tối đa?

```js
Number.MAX_VALUE === (Number.MAX_VALUE + 1);
// true -- oops!

Number.MAX_VALUE === (Number.MAX_VALUE + 10000000);
// true
```

Vì vậy, `Number.MAX_VALUE` có thực sự là giá trị lớn nhất có thể biểu diễn trong JS không? Nó chắc chắn là giá trị `number` *hữu hạn* lớn nhất.

IEEE-754 định nghĩa một giá trị vô hạn đặc biệt, mà JS hiển thị dưới dạng `Infinity`; cũng có một `-Infinity` ở phía đầu kia của trục số. Các giá trị có thể được kiểm tra xem chúng có hữu hạn hay vô hạn không:

```js
Number.isFinite(Number.MAX_VALUE);  // true

Number.isFinite(Infinity);          // false
Number.isFinite(-Infinity);         // false
```

Bạn không bao giờ có thể đếm lên (với `+ 1`) từ `Number.MAX_VALUE` đến `Infinity`, bất kể bạn để chương trình chạy bao lâu, bởi vì phép toán `+ 1` không thực sự tăng vượt quá giá trị `Number.MAX_VALUE` trên cùng.

Tuy nhiên, các phép toán số học JS (`+`, `*`, và thậm chí `/`) chắc chắn có thể làm tràn kiểu `number` ở đầu trên, trong trường hợp đó `Infinity` là kết quả:

```js
Number.MAX_VALUE + 1E291;           // 1.7976931348623157e+308
Number.MAX_VALUE + 1E292;           // Infinity

Number.MAX_VALUE * 1.0000000001;    // Infinity

1 / 1E-308;                         // 1e+308
1 / 1E-309;                         // Infinity
```

| MẸO: |
| :--- |
| Điều ngược lại không đúng: một phép toán số học trên một giá trị vô hạn *sẽ không bao giờ* tạo ra một giá trị hữu hạn. |

Đi từ rất lớn đến rất, rất nhỏ -- thực ra, gần nhất với số không, điều này không giống với việc đi rất, rất âm! -- giá trị thập phân tuyệt đối nhỏ nhất bạn có thể lưu trữ theo lý thuyết trong kiểu `number` sẽ là `2^-1022` (hãy nhớ phạm vi số mũ IEEE-754?), hoặc khoảng `2E-308`. Tuy nhiên, các công cụ JS được phép bởi đặc tả thay đổi trong các biểu diễn nội bộ của chúng cho giới hạn dưới này. Bất kể giới hạn dưới hiệu quả của công cụ là gì, nó sẽ được hiển thị dưới dạng `Number.MIN_VALUE`:

```js
Number.MIN_VALUE;               // 5e-324 <-- thường là vậy!
```

Hầu hết các công cụ JS dường như có một giá trị tối thiểu có thể biểu diễn khoảng `5E-324` (khoảng `2^-1074`). Tùy thuộc vào công cụ và/hoặc nền tảng, một giá trị khác có thể được hiển thị. Hãy cẩn thận về bất kỳ logic chương trình nào dựa vào các giá trị phụ thuộc vào triển khai như vậy.

### Các Giới Hạn Số Nguyên An Toàn

Vì `Number.MAX_VALUE` là một số nguyên, bạn có thể cho rằng đó là số nguyên lớn nhất trong ngôn ngữ. Nhưng điều đó không thực sự chính xác.

Số nguyên lớn nhất bạn có thể lưu trữ chính xác trong kiểu `number` là `2^53 - 1`, hoặc `9007199254740991`, *nhỏ hơn nhiều* so với `Number.MAX_VALUE` (khoảng `2^1024 - 1`). Giá trị an toàn hơn đặc biệt này được hiển thị dưới dạng `Number.MAX_SAFE_INTEGER`:

```js
maxInt = Number.MAX_SAFE_INTEGER;

maxInt;             // 9007199254740991

maxInt + 1;         // 9007199254740992

maxInt + 2;         // 9007199254740992
```

Chúng ta đã thấy các số nguyên lớn hơn `9007199254740991` có thể xuất hiện. Tuy nhiên, những số nguyên lớn hơn đó không "an toàn", ở chỗ độ chính xác bắt đầu bị phá vỡ khi bạn thực hiện các phép toán với chúng. Như được hiển thị ở trên, các biểu thức `maxInt + 1` và `maxInt + 2` đều cho kết quả sai giống nhau, minh họa mối nguy hiểm khi vượt quá giới hạn `Number.MAX_SAFE_INTEGER`.

Nhưng số nguyên an toàn nhỏ nhất là bao nhiêu?

Tùy thuộc vào cách bạn diễn giải "nhỏ nhất", bạn có thể trả lời `0` hoặc... `Number.MIN_SAFE_INTEGER`:

```js
Number.MIN_SAFE_INTEGER;    // -9007199254740991
```

Và JS cung cấp một tiện ích để xác định xem một giá trị có phải là một số nguyên trong phạm vi an toàn này (`-2^53 + 1` - `2^53 - 1`) hay không:

```js
Number.isSafeInteger(2 ** 53);      // false
Number.isSafeInteger(2 ** 53 - 1);  // true
```

### Các Số Không Kép (Double Zeros)

Có thể làm bạn ngạc nhiên khi biết rằng JS có hai số không: `0`, và `-0` (số không âm). Nhưng trên trái đất này "số không âm" là cái gì? [^SignedZero] Một nhà toán học chắc chắn sẽ chần chừ trước một khái niệm như vậy.

Đây không chỉ là một điều kỳ quặc vui nhộn của JS; nó được bắt buộc bởi đặc tả IEEE-754[^IEEE754]. Tất cả các số dấu phẩy động đều có dấu, bao gồm cả số không. Và mặc dù JS có phần che giấu sự tồn tại của `-0`, hoàn toàn có thể tạo ra nó và phát hiện nó:

```js
function isNegZero(v) {
    return v == 0 && (1 / v) == -Infinity;
}

regZero = 0 / 1;
negZero = 0 / -1;

regZero === negZero;        // true -- oops!
Object.is(-0,regZero);      // false -- phù!
Object.is(-0,negZero);      // true

isNegZero(regZero);         // false
isNegZero(negZero);         // true
```

Bạn có thể tự hỏi tại sao chúng ta lại cần một thứ như `-0`. Nó có thể hữu ích khi sử dụng các con số để biểu diễn cả độ lớn của chuyển động (tốc độ) của một vật phẩm nào đó (như nhân vật trò chơi hoặc hoạt hình) và cả hướng của nó (ví dụ: âm = trái, dương = phải).

Nếu không có giá trị số không có dấu, bạn không thể biết vật phẩm đó đang trỏ về hướng nào ngay tại thời điểm nó dừng lại.

| LƯU Ý: |
| :--- |
| Trong khi JS định nghĩa một số không có dấu trong kiểu `number`, không có số không có dấu tương ứng trong kiểu số `bigint`. Như vậy, `-0n` chỉ được diễn giải là `0n`, và cả hai là không thể phân biệt. |

### Số Không Hợp Lệ

Các phép toán toán học đôi khi có thể tạo ra một kết quả không hợp lệ. Ví dụ:

```js
42 / "Kyle";            // NaN
```

Có lẽ là hiển nhiên, nhưng nếu bạn cố gắng chia một số cho một chuỗi, đó là một phép toán toán học không hợp lệ.

Một loại phép toán số không hợp lệ khác là cố gắng chuyển đổi ép buộc một giá trị không giống số thành một `number`. Như đã thảo luận trước đó, chúng ta có thể làm như vậy với hàm `Number(..)` hoặc toán tử một ngôi `+`:

```js
myAge = Number("just a number");

myAge;                  // NaN

+undefined;             // NaN
```

Tất cả các hoạt động không hợp lệ như vậy (toán học hoặc ép buộc/số) tạo ra giá trị `number` đặc biệt gọi là `NaN`.

Nguồn gốc lịch sử của "NaN" (từ đặc tả IEEE-754[^IEEE754]) là viết tắt cho "Not a Number". Về mặt kỹ thuật, có khoảng 9 triệu tỷ giá trị trong không gian số IEEE-754 64-bit được chỉ định là "NaN", nhưng JS xử lý tất cả chúng không thể phân biệt như là giá trị `NaN` duy nhất.

Thật không may, ý nghĩa *không phải là một số* đó tạo ra sự nhầm lẫn, vì `NaN` *hoàn toàn* là một `number`.

| MẸO: |
| :--- |
| Tại sao `NaN` là một `number`?!? Hãy nghĩ về điều ngược lại: điều gì sẽ xảy ra nếu một phép toán toán học/số, như `+` hoặc `/`, tạo ra một giá trị không phải `number` (như `null`, `undefined`, v.v.)? Điều đó sẽ không thực sự lạ và bất ngờ sao? Điều gì sẽ xảy ra nếu chúng ném ra các ngoại lệ, để bạn phải `try..catch` tất cả toán học của mình? Hành vi hợp lý duy nhất là, các phép toán số/toán học *luôn luôn* tạo ra một `number`, ngay cả khi giá trị đó không hợp lệ vì nó đến từ một hoạt động không hợp lệ. |

Để tránh sự nhầm lẫn như vậy, tôi thực sự thích định nghĩa "NaN" là bất kỳ điều nào sau đây thay thế:

* "iNvalid Number" (Số Không Hợp Lệ)
* "Not actual Number" (Không phải Số thực tế)
* "Not available Number" (Số Không có sẵn)
* "Not applicable Number" (Số Không áp dụng được)

`NaN` là một giá trị đặc biệt trong JS, ở chỗ nó là giá trị duy nhất trong ngôn ngữ thiếu *tính chất danh tính* (identity property) -- nó không bao giờ bằng chính nó.

```js
NaN === NaN;            // false
```

Vì vậy, thật không may, toán tử `===` không thể kiểm tra một giá trị để xem nó có phải là `NaN` không. Nhưng có một số cách để làm như vậy:

```js
politicianIQ = "nothing" / Infinity;

Number.isNaN(politicianIQ);         // true

Object.is(NaN,politicianIQ);        // true
[ NaN ].includes(politicianIQ);     // true
```

Đây là một thực tế của hầu như tất cả các chương trình JS, cho dù bạn có nhận ra hay không: `NaN` xảy ra. Nghiêm túc mà nói, gần như tất cả các chương trình thực hiện bất kỳ toán học hoặc chuyển đổi số nào đều có thể gặp `NaN`.

Nếu bạn không kiểm tra `NaN` đúng cách trong các chương trình của mình nơi bạn thực hiện toán học hoặc chuyển đổi số, tôi có thể nói với một mức độ chắc chắn nào đó: bạn có thể có một lỗi số trong chương trình của mình ở đâu đó, và nó chỉ chưa cắn bạn thôi (mà bạn biết!).

| CẢNH BÁO: |
| :--- |
| JS ban đầu cung cấp một hàm toàn cục gọi là `isNaN(..)` để kiểm tra `NaN`, nhưng thật không may nó có một lỗi ép buộc tồn tại lâu đời. `isNaN("Kyle")` trả về `true`, mặc dù giá trị chuỗi `"Kyle"` chắc chắn *không* phải là giá trị `NaN`. Điều này là do hàm `isNaN(..)` toàn cục buộc bất kỳ đối số nào không phải `number` phải ép buộc thành một `number` trước, trước khi kiểm tra `NaN`. Ép buộc `"Kyle"` thành một `number` tạo ra `NaN`, vì vậy bây giờ hàm nhìn thấy một `NaN` và trả về `true`! Hàm `isNaN(..)` toàn cục bị lỗi này vẫn tồn tại trong JS, nhưng không bao giờ nên được sử dụng. Khi kiểm tra `NaN`, luôn sử dụng `Number.isNaN(..)`, `Object.is(..)`, v.v. |

## Các Giá Trị BigInteger

Vì số nguyên an toàn tối đa trong các `number` JS là `9007199254740991` (xem ở trên), giới hạn tương đối thấp như vậy có thể gây ra vấn đề nếu một chương trình JS cần thực hiện toán học số nguyên lớn hơn, hoặc thậm chí chỉ giữ các giá trị như ID số nguyên 64-bit (ví dụ: ID Twitter Tweet).

Vì lý do đó, JS cung cấp kiểu `bigint` thay thế (BigInteger), có thể lưu trữ các số nguyên lớn tùy ý (về mặt lý thuyết không giới hạn, ngoại trừ bởi bộ nhớ máy hữu hạn và/hoặc triển khai JS).

Để phân biệt một `bigint` với một giá trị `number` nguyên (integer), mà nếu không sẽ trông giống nhau (`42`), JS yêu cầu một hậu tố `n` trên các giá trị `bigint`:

```js
myAge = 42n;        // đây là một bigint, không phải number

myKidsAge = 11;     // đây là một number, không phải bigint
```

Hãy minh họa sự không giới hạn trên của `bigint`:

```js
Number.MAX_SAFE_INTEGER;        // 9007199254740991

Number.MAX_SAFE_INTEGER + 2;    // 9007199254740992 -- oops!

myBigInt = 9007199254740991n;

myBigInt + 2n;                  // 9007199254740993n -- phù!

myBigInt ** 2n;                 // 81129638414606663681390495662081n
```

Như bạn có thể thấy, kiểu giá trị `bigint` có thể thực hiện số học chính xác trên giới hạn số nguyên của kiểu giá trị `number`.

| CẢNH BÁO: |
| :--- |
| Nhận thấy rằng toán tử `+` yêu cầu `.. + 2n` thay vì chỉ `.. + 2`? Bạn không thể trộn lẫn các kiểu giá trị `number` và `bigint` trong cùng một biểu thức. Hạn chế này gây phiền nhiễu, nhưng nó bảo vệ chương trình của bạn khỏi các phép toán toán học không hợp lệ sẽ đưa ra các kết quả không mong đợi không rõ ràng. |

Một giá trị `bigint` cũng có thể được tạo bằng hàm `BigInt(..)`; ví dụ, để chuyển đổi một giá trị `number` nguyên (integer) thành một `bigint`:

```js
myAge = 42n;

inc = 1;

myAge += BigInt(inc);

myAge;              // 43n
```

| CẢNH BÁO: |
| :--- |
| Mặc dù có vẻ phản trực giác đối với một số độc giả, `BigInt(..)` *luôn luôn* được gọi mà không có từ khóa `new`. Nếu `new` được sử dụng, một ngoại lệ sẽ bị ném ra. |

Đó chắc chắn là một trong những cách sử dụng phổ biến nhất của hàm `BigInt(..)`: để chuyển đổi các `number` thành các `bigint`, cho các mục đích hoạt động toán học.

Nhưng cũng không quá hiếm khi biểu diễn các giá trị số nguyên lớn dưới dạng chuỗi, đặc biệt nếu các giá trị đó đến môi trường JS từ các môi trường ngôn ngữ khác, hoặc thông qua các định dạng trao đổi nhất định, mà bản thân chúng không hỗ trợ các giá trị kiểu `bigint`.

Như vậy, `BigInt(..)` hữu ích để ép buộc các giá trị chuỗi đó thành các `bigint`:

```js
myBigInt = BigInt("12345678901234567890");

myBigInt;                       // 12345678901234567890n
```

Không giống như `parseInt(..)`, nếu bất kỳ ký tự nào trong chuỗi là không phải số (các chữ số `0-9` hoặc `-`), bao gồm `.` hoặc thậm chí một ký tự hậu tố `n` ở cuối, một ngoại lệ sẽ bị ném ra. Nói cách khác, `BigInt(..)` là một chuyển đổi ép buộc tất cả hoặc không có gì, không phải là một chuyển đổi phân tích cú pháp.

| LƯU Ý: |
| :--- |
| Tôi nghĩ thật vô lý khi `BigInt(..)` sẽ không chấp nhận ký tự `n` ở cuối trong khi ép buộc chuỗi (và do đó bỏ qua nó một cách hiệu quả). Tôi đã vận động kịch liệt cho hành vi đó, trong quy trình TC39, nhưng cuối cùng đã bị từ chối. Theo ý kiến của tôi, bây giờ nó là một cái mụn cóc nhỏ bé trên JS, nhưng dù sao cũng là một cái mụn cóc. |

## Các Giá Trị Symbol

Kiểu `symbol` chứa các giá trị mờ đặc biệt gọi là "symbols". Các giá trị này chỉ có thể được tạo bởi hàm `Symbol(..)`:

```js
secret = Symbol("my secret");
```

| CẢNH BÁO: |
| :--- |
| Cũng giống như với `BigInt(..)`, hàm `Symbol(..)` phải được gọi mà không có từ khóa `new`. |

Chuỗi `"my secret"` được truyền vào lệnh gọi hàm `Symbol(..)` *không* phải là bản thân giá trị symbol, ngay cả khi nó có vẻ như vậy. Nó chỉ đơn thuần là một nhãn mô tả tùy chọn, chỉ được sử dụng cho mục đích gỡ lỗi vì lợi ích của nhà phát triển.

Giá trị bên dưới được trả về từ `Symbol(..)` là một loại giá trị đặc biệt chống lại việc chương trình/nhà phát triển kiểm tra bất cứ điều gì về biểu diễn bên dưới của nó. Đó là những gì tôi muốn nói là "mờ" (opaque - không trong suốt).

| LƯU Ý: |
| :--- |
| Bạn có thể nghĩ về các symbol như thể chúng là các số nguyên tăng đơn điệu -- thực sự, đó là tương tự như cách ít nhất một số công cụ JS triển khai chúng. Nhưng công cụ JS sẽ không bao giờ hiển thị bất kỳ biểu diễn nào về giá trị bên dưới của một symbol theo bất kỳ cách nào mà bạn hoặc chương trình có thể nhìn thấy. |

Các symbol được đảm bảo bởi công cụ JS là duy nhất (chỉ trong chính chương trình), và không thể đoán được. Nói cách khác, một giá trị symbol trùng lặp không bao giờ có thể được tạo ra trong một chương trình.

Bạn có thể đang tự hỏi tại thời điểm này các symbol được sử dụng để làm gì?

Một cách sử dụng điển hình là dưới dạng các giá trị "đặc biệt" mà nhà phát triển phân biệt với bất kỳ giá trị nào khác có thể vô tình va chạm. Ví dụ:

```js
EMPTY = Symbol("not set yet");
myNickname = EMPTY;

// later:

if (myNickname == EMPTY) {
    // ..
}
```

Ở đây, tôi đã định nghĩa một giá trị `EMPTY` đặc biệt và khởi tạo `myNickname` cho nó. Sau đó, tôi kiểm tra xem nó có còn là giá trị đặc biệt đó không, và sau đó thực hiện một số hành động nếu đúng. Tôi có thể không muốn sử dụng `null` hoặc `undefined` cho các mục đích như vậy, vì một nhà phát triển khác có thể truyền vào một trong những giá trị tích hợp chung đó. `EMPTY` ngược lại ở đây là một giá trị duy nhất, không thể đoán được mà chỉ tôi mới có định nghĩa và có quyền kiểm soát và truy cập.

Có lẽ thậm chí phổ biến hơn, các symbol thường được sử dụng làm các thuộc tính đặc biệt (meta-) trên các đối tượng:

```js
myInfo = {
    name: "Kyle Simpson",
    nickname: "getify",
    age: 42
};

// later:
PRIVATE_ID = Symbol("private unique ID, don't touch!");

myInfo[PRIVATE_ID] = generateID();
```

Điều quan trọng cần lưu ý là các thuộc tính symbol vẫn hiển thị công khai trên bất kỳ đối tượng nào; chúng không *thực sự* riêng tư. Nhưng chúng được coi là đặc biệt và tách biệt khỏi bộ sưu tập các thuộc tính đối tượng thông thường. Nó tương tự như nếu tôi đã làm thay thế:

```js
Object.defineProperty(myInfo,"__private_id_dont_touch",{
    value: generateID(),
    enumerable: false,
});
```

Chỉ theo quy ước, hầu hết các nhà phát triển biết rằng nếu một tên thuộc tính được bắt đầu bằng `_` (hoặc thậm chí nhiều hơn thế, `__`!), điều đó có nghĩa là nó là "giả riêng tư" và hãy để yên nó trừ khi họ thực sự phải truy cập nó.

Các symbol về cơ bản phục vụ cùng một trường hợp sử dụng, nhưng tiện dụng hơn một chút so với phương pháp tiếp đầu ngữ.

### Các Symbol Nổi Tiếng (Well-Known Symbols - WKS)

JS định nghĩa trước một tập hợp các symbol, được gọi là *các symbol nổi tiếng* (well-known symbols - WKS), đại diện cho các móc meta-programming đặc biệt nhất định trên các đối tượng. Các symbol này được lưu trữ dưới dạng các thuộc tính tĩnh trên đối tượng hàm `Symbol`. Ví dụ:

```js
myInfo = {
    // ..
};

String(myInfo);         // [object Object]

myInfo[Symbol.toStringTag] = "my-info";
String(myInfo);         // [object my-info]
```

`Symbol.toStringTag` là một symbol nổi tiếng để truy cập và ghi đè biểu diễn chuỗi mặc định của một đối tượng đơn giản (`"[object Object]"`), thay thế phần `"Object"` bằng một giá trị khác (ví dụ, `"my-info"`).

Xem cuốn sách "Objects & Classes" của loạt bài này để biết thêm thông tin về Well-Known Symbols và metaprogramming.

### Sổ Đăng Ký Symbol Toàn Cầu

Thông thường, bạn muốn giữ các giá trị symbol riêng tư, chẳng hạn như bên trong phạm vi mô-đun. Nhưng đôi khi, bạn muốn hiển thị chúng để chúng có thể truy cập toàn cầu trong tất cả các tệp trong một chương trình JS.

Thay vì chỉ gắn chúng như các biến toàn cục (tức là, các thuộc tính trên đối tượng `globalThis`), JS cung cấp một *không gian tên toàn cục* thay thế để đăng ký các symbol trong đó:

```js
// truy xuất nếu đã đăng ký,
// nếu không thì đăng ký
PRIVATE_ID = Symbol.for("private-id");

// elsewhere:

privateIDKey = Symbol.keyFor(PRIVATE_ID);
privateIDKey;           // "private-id"

// elsewhere:

// truy xuất symbol từ sổ đăng ký dưới
// khóa đã chỉ định
privateIDSymbol = Symbol.for(privateIDKey);
```

Giá trị được truyền cho `Symbol.for(..)` *không* giống như được truyền cho `Symbol(..)`. `Symbol.for(..)` mong đợi một *khóa* duy nhất cho symbol được đăng ký dưới nó trong sổ đăng ký toàn cục, trong khi `Symbol(..)` tùy chọn chấp nhận một nhãn mô tả (không nhất thiết phải duy nhất).

Nếu sổ đăng ký không có một symbol dưới *khóa* đã chỉ định đó, một symbol mới (không có nhãn mô tả) được tạo và tự động đăng ký ở đó. Nếu không, `Symbol.for(..)` trả về bất kỳ symbol nào đã đăng ký trước đó dưới *khóa* đó.

Đi theo hướng ngược lại, nếu bạn có chính giá trị symbol, và muốn truy xuất *khóa* mà nó được đăng ký dưới đó, `Symbol.keyFor(..)` lấy chính symbol làm đầu vào, và trả về *khóa* (nếu có). Điều đó hữu ích trong trường hợp thuận tiện hơn khi truyền xung quanh giá trị chuỗi *khóa* hơn là chính symbol.

### Object hay Primitive?

Không giống như các primitive khác như `42`, nơi bạn có thể tạo nhiều bản sao của cùng một giá trị, các symbol *thực sự* hoạt động giống như các tham chiếu đối tượng cụ thể ở chỗ chúng luôn hoàn toàn duy nhất (cho các mục đích gán giá trị và so sánh bằng). Đặc tả cũng phân loại hàm `Symbol()` dưới phần "Fundamental Objects" (Các Đối Tượng Cơ Bản), gọi hàm là một "hàm tạo" (constructor), và thậm chí định nghĩa thuộc tính `prototype` của nó.

Tuy nhiên, như đã đề cập trước đó, `new` không thể được sử dụng với `Symbol(..)`; điều này tương tự như "hàm tạo" `BigInt()`. Chúng ta biết rõ ràng các giá trị `bigint` là các primitive, vì vậy các giá trị `symbol` dường như cùng *loại*.

Và trong phần "Terms and Definitions" (Thuật Ngữ và Định Nghĩa) của đặc tả, nó liệt kê symbol là một giá trị nguyên thủy (primitive value). [^PrimitiveValues] Hơn nữa, chính các giá trị được sử dụng trong các chương trình JS như các primitive thay vì các đối tượng. Ví dụ, các symbol chủ yếu được sử dụng làm khóa trong các đối tượng -- chúng ta biết các đối tượng không thể sử dụng các giá trị đối tượng khác làm khóa! -- cùng với các chuỗi, cũng là các primitive.

Như đã đề cập trước đó, một số công cụ JS thậm chí thực hiện nội bộ các symbol dưới dạng các số nguyên duy nhất, tăng đơn điệu (các primitive!).

Cuối cùng, như đã giải thích ở đầu chương này, chúng ta biết các giá trị nguyên thủy *không được phép* có các thuộc tính được đặt trên chúng, nhưng được *tự động đóng hộp* (auto-boxed) (xem "Automatic Objects" trong Chương 3) nội bộ thành loại object-wrapper tương ứng để tạo điều kiện cho truy cập thuộc tính/phương thức. Các symbol tuân theo tất cả các hành vi chính xác này, giống như tất cả các primitive khác.

Tất cả điều này được xem xét, tôi nghĩ các symbol *giống nhiều hơn* các primitive so với các đối tượng, vì vậy đó là cách tôi trình bày chúng trong cuốn sách này.

## Primitives Là Các Kiểu Tích Hợp Sẵn

Bây giờ chúng ta đã đào sâu vào bảy loại giá trị nguyên thủy (không phải đối tượng) mà JS cung cấp tự động tích hợp sẵn.

Trước khi chúng ta chuyển sang thảo luận về loại giá trị đối tượng tích hợp của JS, chúng ta muốn xem xét kỹ hơn các loại hành vi mà chúng ta có thể mong đợi từ các giá trị JS. Chúng ta sẽ làm như vậy một cách chuyên sâu, trong chương tiếp theo.

[^PrimitiveValues]: "4.4.5 primitive value", ECMAScript 2022 Language Specification; <https://tc39.es/ecma262/#sec-primitive-value> ; Accessed August 2022

[^UTFUCS]: "JavaScript’s internal character encoding: UCS-2 or UTF-16?"; Mathias Bynens; January 20 2012; <https://mathiasbynens.be/notes/javascript-encoding> ; Accessed July 2022

[^IEEE754]: "IEEE-754"; <https://en.wikipedia.org/wiki/IEEE_754> ; Accessed July 2022

[^NumberType]: "6.1.6.1 The Number Type", ECMAScript 2022 Language Specification; <https://262.ecma-international.org/13.0/#sec-ecmascript-language-types-number-type> ; Accessed August 2022

[^SignedZero]: "Signed Zero", Wikipedia; <https://en.wikipedia.org/wiki/Signed_zero> ; Accessed August 2022
