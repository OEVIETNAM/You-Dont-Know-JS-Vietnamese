---
layout: default
title: Chương 1
parent: Phạm Vi & Closures
nav_order: 2
---

# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Chương 1: Phạm Vi Là Gì?

Vào thời điểm bạn đã viết vài chương trình đầu tiên, bạn có thể đã khá thoải mái với việc tạo biến và lưu trữ giá trị trong chúng. Làm việc với biến là một trong những điều cơ bản nhất mà chúng ta làm trong lập trình!

Nhưng bạn có thể chưa xem xét kỹ lưỡng các cơ chế cơ bản được công cụ sử dụng để tổ chức và quản lý các biến này. Tôi không có nghĩa là bộ nhớ được phân bổ như thế nào trên máy tính, mà là: làm thế nào JS biết biến nào có thể truy cập được bởi bất kỳ câu lệnh nào, và nó xử lý hai biến có cùng tên như thế nào?

Câu trả lời cho những câu hỏi như thế này có dạng các quy tắc được định nghĩa rõ ràng gọi là phạm vi (scope). Cuốn sách này sẽ đào sâu vào tất cả các khía cạnh của phạm vi—cách nó hoạt động, nó hữu ích cho việc gì, các cạm bẫy cần tránh—và sau đó chỉ ra các mẫu phạm vi phổ biến hướng dẫn cấu trúc của chương trình.

Bước đầu tiên của chúng ta là khám phá cách công cụ JS xử lý chương trình của chúng ta **trước khi** nó chạy.

## Về Cuốn Sách Này

Chào mừng đến với cuốn sách 2 trong bộ *You Don't Know JS Yet*! Nếu bạn đã hoàn thành *Get Started* (cuốn sách đầu tiên), bạn đang ở đúng chỗ! Nếu chưa, trước khi tiếp tục, tôi khuyến khích bạn *bắt đầu ở đó* để có nền tảng tốt nhất.

Trọng tâm của chúng ta sẽ là trụ cột đầu tiên trong ba trụ cột của ngôn ngữ JS: hệ thống phạm vi và closures của hàm, cũng như sức mạnh của mẫu thiết kế module.

JS thường được phân loại là một ngôn ngữ kịch bản được thông dịch, vì vậy hầu hết mọi người cho rằng các chương trình JS được xử lý trong một lần duyệt từ trên xuống dưới. Nhưng JS thực sự được phân tích cú pháp/biên dịch trong một giai đoạn riêng biệt **trước khi thực thi bắt đầu**. Các quyết định của tác giả mã về nơi đặt biến, hàm và khối liên quan đến nhau được phân tích theo các quy tắc của phạm vi, trong giai đoạn phân tích cú pháp/biên dịch ban đầu. Cấu trúc phạm vi kết quả thường không bị ảnh hưởng bởi các điều kiện thời gian chạy.

Các hàm JS tự chúng là giá trị hạng nhất; chúng có thể được gán và truyền xung quanh giống như số hoặc chuỗi. Nhưng vì các hàm này giữ và truy cập biến, chúng duy trì phạm vi ban đầu của chúng bất kể các hàm cuối cùng được thực thi ở đâu trong chương trình. Điều này được gọi là closure.

Modules là một mẫu tổ chức mã được đặc trưng bởi các phương thức công khai có quyền truy cập đặc quyền (thông qua closure) vào các biến và hàm ẩn trong phạm vi nội bộ của module.

## Biên Dịch so với Thông Dịch

Bạn có thể đã nghe nói về *biên dịch mã* trước đây, nhưng có lẽ nó có vẻ như một hộp đen bí ẩn nơi mã nguồn trượt vào một đầu và các chương trình thực thi bật ra đầu kia.

Tuy nhiên, nó không bí ẩn hay kỳ diệu. Biên dịch mã là một tập hợp các bước xử lý văn bản mã của bạn và biến nó thành một danh sách các hướng dẫn mà máy tính có thể hiểu. Thông thường, toàn bộ mã nguồn được chuyển đổi cùng một lúc, và các hướng dẫn kết quả được lưu dưới dạng đầu ra (thường là trong một tệp) có thể được thực thi sau này.

Bạn cũng có thể đã nghe nói rằng mã có thể được *thông dịch*, vậy điều đó khác với *biên dịch* như thế nào?

Thông dịch thực hiện một nhiệm vụ tương tự như biên dịch, ở chỗ nó chuyển đổi chương trình của bạn thành các hướng dẫn mà máy có thể hiểu. Nhưng mô hình xử lý là khác nhau. Không giống như một chương trình được biên dịch cùng một lúc, với thông dịch, mã nguồn được chuyển đổi từng dòng; mỗi dòng hoặc câu lệnh được thực thi trước khi ngay lập tức tiến hành xử lý dòng tiếp theo của mã nguồn.

<figure>
    <img src="images/fig1.png" width="650" alt="Code Compilation and Code Interpretation" align="center">
    <figcaption><em>Hình 1: Biên Dịch Mã so với Thông Dịch Mã</em></figcaption>
    <br><br>
</figure>

Hình 1 minh họa biên dịch so với thông dịch các chương trình.

Hai mô hình xử lý này có loại trừ lẫn nhau không? Nói chung là có. Tuy nhiên, vấn đề phức tạp hơn, bởi vì thông dịch thực sự có thể có các dạng khác ngoài việc chỉ hoạt động từng dòng trên văn bản mã nguồn. Các công cụ JS hiện đại thực sự sử dụng nhiều biến thể của cả biên dịch và thông dịch trong việc xử lý các chương trình JS.

Hãy nhớ lại rằng chúng ta đã khảo sát chủ đề này trong Chương 1 của cuốn sách *Get Started*. Kết luận của chúng ta ở đó là JS được mô tả chính xác nhất là một **ngôn ngữ biên dịch**. Vì lợi ích của độc giả ở đây, các phần sau sẽ xem xét lại và mở rộng khẳng định đó.

## Biên Dịch Mã

Nhưng trước tiên, tại sao việc JS có được biên dịch hay không lại quan trọng?

Phạm vi chủ yếu được xác định trong quá trình biên dịch, vì vậy việc hiểu cách biên dịch và thực thi liên quan đến nhau là chìa khóa để làm chủ phạm vi.

Trong lý thuyết trình biên dịch cổ điển, một chương trình được xử lý bởi trình biên dịch trong ba giai đoạn cơ bản:

1. **Tokenizing/Lexing (Phân tích từ vựng):** chia một chuỗi ký tự thành các phần có ý nghĩa (đối với ngôn ngữ), được gọi là token. Ví dụ, hãy xem xét chương trình: `var a = 2;`. Chương trình này có thể sẽ được chia thành các token sau: `var`, `a`, `=`, `2`, và `;`. Khoảng trắng có thể được giữ lại hoặc không như một token, tùy thuộc vào việc nó có ý nghĩa hay không.

    (Sự khác biệt giữa tokenizing và lexing là tinh tế và mang tính học thuật, nhưng nó tập trung vào việc liệu các token này có được xác định theo cách *không trạng thái* hay *có trạng thái* hay không. Nói một cách đơn giản, nếu tokenizer gọi các quy tắc phân tích cú pháp có trạng thái để tìm ra xem `a` nên được coi là một token riêng biệt hay chỉ là một phần của token khác, *đó* sẽ là **lexing**.)

2. **Parsing (Phân tích cú pháp):** lấy một luồng (mảng) các token và biến nó thành một cây các phần tử lồng nhau, cùng nhau đại diện cho cấu trúc ngữ pháp của chương trình. Điều này được gọi là Cây Cú pháp Trừu tượng (Abstract Syntax Tree - AST).

    Ví dụ, cây cho `var a = 2;` có thể bắt đầu với một nút cấp cao nhất được gọi là `VariableDeclaration`, với một nút con được gọi là `Identifier` (có giá trị là `a`), và một nút con khác được gọi là `AssignmentExpression` mà bản thân nó có một nút con được gọi là `NumericLiteral` (có giá trị là `2`).

3. **Code Generation (Tạo mã):** lấy một AST và biến nó thành mã thực thi. Phần này thay đổi rất nhiều tùy thuộc vào ngôn ngữ, nền tảng mà nó nhắm đến, và các yếu tố khác.

    Công cụ JS lấy AST vừa mô tả cho `var a = 2;` và biến nó thành một tập hợp các hướng dẫn máy để thực sự *tạo* một biến có tên `a` (bao gồm dự trữ bộ nhớ, v.v.), và sau đó lưu trữ một giá trị vào `a`.

| LƯU Ý: |
| :--- |
| Các chi tiết triển khai của công cụ JS (sử dụng tài nguyên bộ nhớ hệ thống, v.v.) sâu hơn nhiều so với những gì chúng ta sẽ đào sâu ở đây. Chúng ta sẽ giữ trọng tâm của mình vào hành vi có thể quan sát được của các chương trình của chúng ta và để công cụ JS quản lý các trừu tượng cấp hệ thống sâu hơn đó. |

Công cụ JS phức tạp hơn nhiều so với *chỉ* ba giai đoạn này. Trong quá trình phân tích cú pháp và tạo mã, có các bước để tối ưu hóa hiệu suất của việc thực thi (ví dụ: thu gọn các phần tử dư thừa). Trên thực tế, mã thậm chí có thể được biên dịch lại và tối ưu hóa lại trong quá trình thực thi.

Vì vậy, tôi chỉ vẽ bằng những nét rộng ở đây. Nhưng bạn sẽ sớm thấy tại sao *những* chi tiết *chúng ta* đề cập, ngay cả ở cấp độ cao, lại có liên quan.

Các công cụ JS không có sự xa xỉ của thời gian dồi dào để thực hiện công việc và tối ưu hóa của chúng, bởi vì biên dịch JS không xảy ra trong một bước xây dựng trước thời hạn, như với các ngôn ngữ khác. Nó thường phải xảy ra trong vài micro giây (hoặc ít hơn!) ngay trước khi mã được thực thi. Để đảm bảo hiệu suất nhanh nhất trong những ràng buộc này, các công cụ JS sử dụng tất cả các loại thủ thuật (như JIT, biên dịch lười và thậm chí biên dịch lại nóng); những điều này vượt xa "phạm vi" của cuộc thảo luận của chúng ta ở đây.

### Bắt Buộc: Hai Giai Đoạn

Để nói một cách đơn giản nhất có thể, quan sát quan trọng nhất mà chúng ta có thể thực hiện về việc xử lý các chương trình JS là nó xảy ra trong (ít nhất) hai giai đoạn: phân tích cú pháp/biên dịch trước, sau đó là thực thi.

Sự tách biệt của giai đoạn phân tích cú pháp/biên dịch khỏi giai đoạn thực thi tiếp theo là sự thật có thể quan sát được, không phải lý thuyết hay ý kiến. Mặc dù đặc tả JS không yêu cầu "biên dịch" một cách rõ ràng, nó yêu cầu hành vi về cơ bản chỉ thực tế với cách tiếp cận biên dịch-sau đó-thực thi.

Có ba đặc điểm chương trình bạn có thể quan sát để tự chứng minh điều này: lỗi cú pháp, lỗi sớm và hoisting.

#### Lỗi Cú Pháp Từ Đầu

Hãy xem xét chương trình này:

```js
var greeting = "Hello";

console.log(greeting);

greeting = ."Hi";
// SyntaxError: unexpected token .
```

Chương trình này không tạo ra đầu ra (`"Hello"` không được in), mà thay vào đó ném ra một `SyntaxError` về token `.` không mong đợi ngay trước chuỗi `"Hi"`. Vì lỗi cú pháp xảy ra sau câu lệnh `console.log(..)` được định dạng tốt, nếu JS đang thực thi từ trên xuống dưới từng dòng, người ta sẽ mong đợi thông báo `"Hello"` được in trước khi lỗi cú pháp được ném ra. Điều đó không xảy ra.

Trên thực tế, cách duy nhất để công cụ JS có thể biết về lỗi cú pháp trên dòng thứ ba, trước khi thực thi dòng thứ nhất và thứ hai, là công cụ JS trước tiên phân tích cú pháp toàn bộ chương trình trước khi bất kỳ phần nào của nó được thực thi.

#### Lỗi Sớm

Tiếp theo, hãy xem xét:

```js
console.log("Howdy");

saySomething("Hello","Hi");
// Uncaught SyntaxError: Duplicate parameter name not
// allowed in this context

function saySomething(greeting,greeting) {
    "use strict";
    console.log(greeting);
}
```

Thông báo `"Howdy"` không được in, mặc dù là một câu lệnh được định dạng tốt.

Thay vào đó, giống như đoạn mã trong phần trước, `SyntaxError` ở đây được ném ra trước khi chương trình được thực thi. Trong trường hợp này, đó là bởi vì chế độ nghiêm ngặt (được chọn chỉ cho hàm `saySomething(..)` ở đây) cấm, trong số nhiều thứ khác, các hàm có tên tham số trùng lặp; điều này luôn được phép trong chế độ không nghiêm ngặt.

Lỗi được ném ra không phải là lỗi cú pháp theo nghĩa là một chuỗi token bị định dạng sai (như `.\"Hi\"` trước đó), nhưng trong chế độ nghiêm ngặt vẫn được yêu cầu bởi đặc tả để được ném ra như một "lỗi sớm" trước khi bất kỳ thực thi nào bắt đầu.

Nhưng làm thế nào công cụ JS biết rằng tham số `greeting` đã bị trùng lặp? Làm thế nào nó biết rằng hàm `saySomething(..)` thậm chí đang ở chế độ nghiêm ngặt trong khi xử lý danh sách tham số (pragma `"use strict"` chỉ xuất hiện sau này, trong thân hàm)?

Một lần nữa, lời giải thích hợp lý duy nhất là mã phải được *hoàn toàn* phân tích cú pháp trước khi bất kỳ thực thi nào xảy ra.

#### Hoisting

Cuối cùng, hãy xem xét:

```js
function saySomething() {
    var greeting = "Hello";
    {
        greeting = "Howdy";  // lỗi đến từ đây
        let greeting = "Hi";
        console.log(greeting);
    }
}

saySomething();
// ReferenceError: Cannot access 'greeting' before
// initialization
```

`ReferenceError` được ghi chú xảy ra từ dòng với câu lệnh `greeting = "Howdy"`. Điều đang xảy ra là biến `greeting` cho câu lệnh đó thuộc về khai báo trên dòng tiếp theo, `let greeting = "Hi"`, thay vì câu lệnh `var greeting = "Hello"` trước đó.

Cách duy nhất để công cụ JS có thể biết, tại dòng nơi lỗi được ném ra, rằng *câu lệnh tiếp theo* sẽ khai báo một biến phạm vi khối có cùng tên (`greeting`) là nếu công cụ JS đã xử lý mã này trong một lần duyệt trước đó, và đã thiết lập tất cả các phạm vi và các liên kết biến của chúng. Việc xử lý các phạm vi và khai báo này chỉ có thể được thực hiện chính xác bằng cách phân tích cú pháp chương trình trước khi thực thi.

`ReferenceError` ở đây về mặt kỹ thuật đến từ `greeting = "Howdy"` truy cập biến `greeting` **quá sớm**, một xung đột được gọi là Vùng Chết Tạm thời (Temporal Dead Zone - TDZ). Chương 5 sẽ đề cập chi tiết hơn về điều này.

| CẢNH BÁO: |
| :--- |
| Thường được khẳng định rằng các khai báo `let` và `const` không được hoisted, như một lời giải thích cho hành vi TDZ vừa được minh họa. Nhưng điều này không chính xác. Chúng ta sẽ quay lại và giải thích cả hoisting và TDZ của `let`/`const` trong Chương 5. |

Hy vọng bây giờ bạn đã tin rằng các chương trình JS được phân tích cú pháp trước khi bất kỳ thực thi nào bắt đầu. Nhưng liệu nó có chứng minh rằng chúng được biên dịch không?

Đây là một câu hỏi thú vị để suy ngẫm. Liệu JS có thể phân tích cú pháp một chương trình, nhưng sau đó thực thi chương trình đó bằng cách *thông dịch* các hoạt động được đại diện trong AST **mà không** biên dịch chương trình trước không? Có, điều đó là *có thể*. Nhưng nó cực kỳ không chắc, chủ yếu là vì nó sẽ cực kỳ không hiệu quả về mặt hiệu suất.

Thật khó để tưởng tượng một công cụ JS chất lượng sản xuất phải trải qua tất cả rắc rối của việc phân tích cú pháp một chương trình thành một AST, nhưng sau đó không chuyển đổi (hay còn gọi là "biên dịch") AST đó thành biểu diễn (nhị phân) hiệu quả nhất để công cụ sau đó thực thi.

Nhiều người đã cố gắng chia nhỏ thuật ngữ này, vì có rất nhiều sắc thái và "thực ra..." xen vào xung quanh. Nhưng về tinh thần và trong thực tế, những gì công cụ đang làm trong việc xử lý các chương trình JS **giống biên dịch hơn nhiều** so với không.

Phân loại JS là một ngôn ngữ biên dịch không liên quan đến mô hình phân phối cho các biểu diễn thực thi nhị phân (hoặc byte-code) của nó, mà là giữ một sự phân biệt rõ ràng trong tâm trí của chúng ta về giai đoạn mà mã JS được xử lý và phân tích; giai đoạn này có thể quan sát được và không thể tranh cãi xảy ra *trước khi* mã bắt đầu được thực thi.

Chúng ta cần các mô hình tinh thần phù hợp về cách công cụ JS xử lý mã của chúng ta nếu chúng ta muốn hiểu JS và phạm vi một cách hiệu quả.

## Ngôn Ngữ Trình Biên Dịch

Với nhận thức về xử lý hai giai đoạn của chương trình JS (biên dịch, sau đó thực thi), hãy chuyển sự chú ý của chúng ta sang cách công cụ JS xác định các biến và xác định các phạm vi của một chương trình khi nó được biên dịch.

Đầu tiên, hãy kiểm tra một chương trình JS đơn giản để sử dụng cho phân tích trong vài chương tiếp theo:

```js
var students = [
    { id: 14, name: "Kyle" },
    { id: 73, name: "Suzy" },
    { id: 112, name: "Frank" },
    { id: 6, name: "Sarah" }
];

function getStudentName(studentID) {
    for (let student of students) {
        if (student.id == studentID) {
            return student.name;
        }
    }
}

var nextStudent = getStudentName(73);

console.log(nextStudent);
// Suzy
```

Ngoài các khai báo, tất cả các lần xuất hiện của biến/định danh trong một chương trình phục vụ trong một trong hai "vai trò": hoặc chúng là *mục tiêu* của một phép gán hoặc chúng là *nguồn* của một giá trị.

(Khi tôi lần đầu tiên học lý thuyết trình biên dịch trong khi lấy bằng khoa học máy tính, chúng tôi được dạy các thuật ngữ "LHS" (hay còn gọi là *mục tiêu*) và "RHS" (hay còn gọi là *nguồn*) cho các vai trò này, tương ứng. Như bạn có thể đoán từ "L" và "R", các từ viết tắt có nghĩa là "Left-Hand Side" (Bên Trái) và "Right-Hand Side" (Bên Phải), như trong bên trái và bên phải của toán tử gán `=`. Tuy nhiên, các mục tiêu và nguồn gán không phải lúc nào cũng xuất hiện theo nghĩa đen ở bên trái hoặc bên phải của `=`, vì vậy có lẽ rõ ràng hơn khi nghĩ về *mục tiêu* / *nguồn* thay vì *trái* / *phải*.)

Làm thế nào để bạn biết nếu một biến là *mục tiêu*? Kiểm tra xem có giá trị nào đang được gán cho nó không; nếu có, đó là *mục tiêu*. Nếu không, thì biến là *nguồn*.

Để công cụ JS xử lý đúng các biến của chương trình, trước tiên nó phải gắn nhãn mỗi lần xuất hiện của một biến là *mục tiêu* hoặc *nguồn*. Bây giờ chúng ta sẽ đào sâu vào cách mỗi vai trò được xác định.

### Mục Tiêu

Điều gì làm cho một biến trở thành *mục tiêu*? Hãy xem xét:

```js
students = [ // ..
```

Câu lệnh này rõ ràng là một hoạt động gán; hãy nhớ rằng, phần `var students` được xử lý hoàn toàn như một khai báo tại thời điểm biên dịch, và do đó không liên quan trong quá trình thực thi; chúng tôi đã bỏ nó ra để rõ ràng và tập trung. Tương tự với câu lệnh `nextStudent = getStudentName(73)`.

Nhưng có ba hoạt động gán *mục tiêu* khác trong mã có thể ít rõ ràng hơn. Một trong số chúng:

```js
for (let student of students) {
```

Câu lệnh đó gán một giá trị cho `student` cho mỗi lần lặp của vòng lặp. Một tham chiếu *mục tiêu* khác:

```js
getStudentName(73)
```

Nhưng làm thế nào đó là một phép gán cho một *mục tiêu*? Hãy nhìn kỹ: đối số `73` được gán cho tham số `studentID`.

Và có một tham chiếu *mục tiêu* cuối cùng (tinh tế) trong chương trình của chúng ta. Bạn có thể phát hiện nó không?

..

..

..

Bạn đã xác định cái này chưa?

```js
function getStudentName(studentID) {
```

Một khai báo `function` là một trường hợp đặc biệt của tham chiếu *mục tiêu*. Bạn có thể nghĩ về nó giống như `var getStudentName = function(studentID)`, nhưng điều đó không hoàn toàn chính xác. Một định danh `getStudentName` được khai báo (tại thời điểm biên dịch), nhưng phần `= function(studentID)` cũng được xử lý tại thời điểm biên dịch; sự liên kết giữa `getStudentName` và hàm được thiết lập tự động ở đầu phạm vi thay vì chờ đợi một câu lệnh gán `=` được thực thi.

| LƯU Ý: |
| :--- |
| Sự liên kết tự động này của hàm và biến được gọi là "function hoisting", và được đề cập chi tiết trong Chương 5. |

### Nguồn

Vì vậy, chúng ta đã xác định tất cả năm tham chiếu *mục tiêu* trong chương trình. Các tham chiếu biến khác sau đó phải là tham chiếu *nguồn* (bởi vì đó là tùy chọn duy nhất khác!).

Trong `for (let student of students)`, chúng ta đã nói rằng `student` là *mục tiêu*, nhưng `students` là tham chiếu *nguồn*. Trong câu lệnh `if (student.id == studentID)`, cả `student` và `studentID` đều là tham chiếu *nguồn*. `student` cũng là tham chiếu *nguồn* trong `return student.name`.

Trong `getStudentName(73)`, `getStudentName` là tham chiếu *nguồn* (mà chúng ta hy vọng phân giải thành giá trị tham chiếu hàm). Trong `console.log(nextStudent)`, `console` là tham chiếu *nguồn*, cũng như `nextStudent`.

| LƯU Ý: |
| :--- |
| Trong trường hợp bạn tự hỏi, `id`, `name`, và `log` đều là thuộc tính, không phải tham chiếu biến. |

Tầm quan trọng thực tế của việc hiểu *mục tiêu* so với *nguồn* là gì? Trong Chương 2, chúng ta sẽ xem lại chủ đề này và đề cập đến cách vai trò của biến ảnh hưởng đến tra cứu của nó (cụ thể, nếu tra cứu thất bại).

## Gian Lận: Sửa Đổi Phạm Vi Thời Gian Chạy

Bây giờ nó nên rõ ràng rằng phạm vi được xác định khi chương trình được biên dịch, và thường không bị ảnh hưởng bởi các điều kiện thời gian chạy. Tuy nhiên, trong chế độ không nghiêm ngặt, về mặt kỹ thuật vẫn có hai cách để gian lận quy tắc này, sửa đổi các phạm vi của chương trình trong thời gian chạy.

Không nên sử dụng cả hai kỹ thuật này—cả hai đều nguy hiểm và khó hiểu, và bạn nên sử dụng chế độ nghiêm ngặt (nơi chúng bị cấm) dù sao đi nữa. Nhưng điều quan trọng là phải nhận thức được chúng trong trường hợp bạn gặp chúng trong một số chương trình.

Hàm `eval(..)` nhận một chuỗi mã để biên dịch và thực thi ngay lập tức trong thời gian chạy chương trình. Nếu chuỗi mã đó có khai báo `var` hoặc `function` trong đó, các khai báo đó sẽ sửa đổi phạm vi hiện tại mà `eval(..)` hiện đang thực thi trong:

```js
function badIdea() {
    eval("var oops = 'Ugh!';");
    console.log(oops);
}
badIdea();   // Ugh!
```

Nếu `eval(..)` không có mặt, biến `oops` trong `console.log(oops)` sẽ không tồn tại, và sẽ ném ra một `ReferenceError`. Nhưng `eval(..)` sửa đổi phạm vi của hàm `badIdea()` tại thời gian chạy. Điều này tệ vì nhiều lý do, bao gồm cả tác động hiệu suất của việc sửa đổi phạm vi đã được biên dịch và tối ưu hóa, mỗi khi `badIdea()` chạy.

Gian lận thứ hai là từ khóa `with`, về cơ bản biến một đối tượng thành phạm vi cục bộ một cách động—các thuộc tính của nó được coi như các định danh trong khối phạm vi mới đó:

```js
var badIdea = { oops: "Ugh!" };

with (badIdea) {
    console.log(oops);   // Ugh!
}
```

Phạm vi toàn cục không bị sửa đổi ở đây, nhưng `badIdea` đã được biến thành một phạm vi tại thời gian chạy thay vì thời gian biên dịch, và thuộc tính `oops` của nó trở thành một biến trong phạm vi đó. Một lần nữa, đây là một ý tưởng tồi tệ, vì lý do hiệu suất và khả năng đọc.

Bằng mọi giá, hãy tránh `eval(..)` (ít nhất, `eval(..)` tạo khai báo) và `with`. Một lần nữa, không có gian lận nào trong số này có sẵn trong chế độ nghiêm ngặt, vì vậy nếu bạn chỉ sử dụng chế độ nghiêm ngặt (bạn nên!), thì sự cám dỗ sẽ biến mất!

## Phạm Vi Từ Vựng

Chúng ta đã chứng minh rằng phạm vi của JS được xác định tại thời điểm biên dịch; thuật ngữ cho loại phạm vi này là "phạm vi từ vựng" (lexical scope). "Từ vựng" (Lexical) được liên kết với giai đoạn "lexing" của biên dịch, như đã thảo luận trước đó trong chương này.

Để thu hẹp chương này xuống một kết luận hữu ích, ý tưởng chính của "phạm vi từ vựng" là nó được kiểm soát hoàn toàn bởi vị trí của các hàm, khối và khai báo biến, liên quan đến nhau.

Nếu bạn đặt một khai báo biến bên trong một hàm, trình biên dịch xử lý khai báo này khi nó phân tích cú pháp hàm, và liên kết khai báo đó với phạm vi của hàm. Nếu một biến được khai báo phạm vi khối (`let` / `const`), thì nó được liên kết với khối `{ .. }` bao quanh gần nhất, thay vì hàm bao quanh của nó (như với `var`).

Hơn nữa, một tham chiếu (vai trò *mục tiêu* hoặc *nguồn*) cho một biến phải được phân giải là đến từ một trong các phạm vi *có sẵn từ vựng* cho nó; nếu không, biến được cho là "chưa được khai báo" (thường dẫn đến lỗi!). Nếu biến không được khai báo trong phạm vi hiện tại, phạm vi bên ngoài/bao quanh tiếp theo sẽ được tham khảo. Quá trình bước ra một cấp độ lồng phạm vi này tiếp tục cho đến khi có thể tìm thấy một khai báo biến khớp, hoặc phạm vi toàn cục được đạt đến và không còn nơi nào khác để đi.

Điều quan trọng cần lưu ý là biên dịch thực sự không *làm bất cứ điều gì* về mặt dự trữ bộ nhớ cho các phạm vi và biến. Không có phần nào của chương trình đã được thực thi.

Thay vào đó, biên dịch tạo ra một bản đồ của tất cả các phạm vi từ vựng đặt ra những gì chương trình sẽ cần trong khi nó thực thi. Bạn có thể nghĩ về kế hoạch này như mã được chèn để sử dụng tại thời gian chạy, xác định tất cả các phạm vi (hay còn gọi là "môi trường từ vựng") và đăng ký tất cả các định danh (biến) cho mỗi phạm vi.

Nói cách khác, trong khi các phạm vi được xác định trong quá trình biên dịch, chúng không thực sự được tạo ra cho đến thời gian chạy, mỗi khi một phạm vi cần chạy. Trong chương tiếp theo, chúng ta sẽ phác thảo các nền tảng khái niệm cho phạm vi từ vựng.
