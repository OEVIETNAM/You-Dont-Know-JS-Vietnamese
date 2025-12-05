# You Don't Know JS Yet: Bắt đầu - Ấn bản thứ 2
# Chương 2: Khảo sát JS

Cách tốt nhất để học JS là bắt đầu viết JS.

Để làm điều đó, bạn cần biết ngôn ngữ hoạt động như thế nào, và đó là những gì chúng ta sẽ tập trung vào đây. Ngay cả khi bạn đã lập trình bằng các ngôn ngữ khác trước đây, hãy dành thời gian để làm quen với JS và đảm bảo thực hành từng phần.

Chương này không phải là tài liệu tham khảo đầy đủ về mọi cú pháp của ngôn ngữ JS. Nó cũng không nhằm mục đích trở thành một cuốn sách "nhập môn JS" hoàn chỉnh.

Thay vào đó, chúng ta sẽ chỉ khảo sát một số lĩnh vực chủ đề chính của ngôn ngữ. Mục tiêu của chúng ta là có được *cảm nhận* tốt hơn về nó, để chúng ta có thể tiến tới viết các chương trình của riêng mình với sự tự tin hơn. Chúng ta sẽ xem lại nhiều chủ đề trong số này chi tiết hơn khi bạn đi qua phần còn lại của cuốn sách này và phần còn lại của bộ sách.

Xin đừng mong đợi chương này là một bài đọc nhanh. Nó dài và có rất nhiều chi tiết để nghiền ngẫm. Hãy dành thời gian của bạn.

| MẸO: |
| :--- |
| Nếu bạn vẫn đang làm quen với JS, tôi khuyên bạn nên dành nhiều thời gian thêm để làm việc qua chương này. Hãy dành thời gian cho mỗi phần và suy ngẫm cũng như khám phá chủ đề trong một thời gian. Xem qua các chương trình JS hiện có và so sánh những gì bạn thấy trong đó với mã và giải thích (và ý kiến!) được trình bày ở đây. Bạn sẽ nhận được nhiều hơn từ phần còn lại của cuốn sách và bộ sách với một nền tảng vững chắc về *bản chất* của JS. |

## Mỗi tệp là một chương trình

Hầu hết mọi trang web (ứng dụng web) bạn sử dụng đều bao gồm nhiều tệp JS khác nhau (thường có phần mở rộng tệp .js). Thật hấp dẫn khi nghĩ về toàn bộ mọi thứ (ứng dụng) như một chương trình. Nhưng JS nhìn nhận nó khác đi.

Trong JS, mỗi tệp độc lập là một chương trình riêng biệt của chính nó.

Lý do điều này quan trọng chủ yếu xoay quanh việc xử lý lỗi. Vì JS coi các tệp là các chương trình, một tệp có thể bị lỗi (trong quá trình phân tích cú pháp/biên dịch hoặc thực thi) và điều đó không nhất thiết ngăn cản tệp tiếp theo được xử lý. Rõ ràng, nếu ứng dụng của bạn phụ thuộc vào năm tệp .js và một trong số chúng bị lỗi, ứng dụng tổng thể có thể sẽ chỉ hoạt động một phần, tốt nhất là vậy. Điều quan trọng là đảm bảo rằng mỗi tệp hoạt động bình thường và ở mức độ nào đó có thể, chúng xử lý lỗi trong các tệp khác một cách duyên dáng nhất có thể.

Bạn có thể ngạc nhiên khi coi các tệp .js riêng biệt là các chương trình JS riêng biệt. Từ góc độ sử dụng ứng dụng của bạn, chắc chắn nó có vẻ giống như một chương trình lớn. Đó là bởi vì việc thực thi ứng dụng cho phép các *chương trình* riêng lẻ này hợp tác và hoạt động như một chương trình.

| LƯU Ý: |
| :--- |
| Nhiều dự án sử dụng các công cụ quy trình build cuối cùng kết hợp các tệp riêng biệt từ dự án thành một tệp duy nhất để được chuyển đến một trang web. Khi điều này xảy ra, JS coi tệp kết hợp duy nhất này là toàn bộ chương trình. |

Cách duy nhất nhiều tệp .js độc lập hoạt động như một chương trình duy nhất là chia sẻ trạng thái của chúng (và quyền truy cập vào chức năng công khai của chúng) thông qua "phạm vi toàn cục" (global scope). Chúng trộn lẫn với nhau trong không gian tên phạm vi toàn cục này, vì vậy khi runtime chúng hoạt động như một thể thống nhất.

Kể từ ES6, JS cũng đã hỗ trợ định dạng mô-đun bên cạnh định dạng chương trình JS độc lập điển hình. Các mô-đun cũng dựa trên tệp. Nếu một tệp được tải thông qua cơ chế tải mô-đun như câu lệnh `import` hoặc thẻ `<script type=module>`, tất cả mã của nó được coi là một mô-đun duy nhất.

Mặc dù bạn thường không nghĩ về một mô-đun—một tập hợp trạng thái và các phương thức được hiển thị công khai để hoạt động trên trạng thái đó—như một chương trình độc lập, JS thực tế vẫn xử lý mỗi mô-đun một cách riêng biệt. Tương tự như cách "phạm vi toàn cục" cho phép các tệp độc lập trộn lẫn với nhau khi runtime, việc nhập một mô-đun vào một mô-đun khác cho phép khả năng tương tác runtime giữa chúng.

Bất kể mô hình tổ chức mã (và cơ chế tải) nào được sử dụng cho một tệp (độc lập hoặc mô-đun), bạn vẫn nên nghĩ về mỗi tệp như một chương trình (mini) của riêng nó, sau đó có thể hợp tác với các chương trình (mini) khác để thực hiện các chức năng của ứng dụng tổng thể của bạn.

## Giá trị

Đơn vị thông tin cơ bản nhất trong một chương trình là một giá trị. Giá trị là dữ liệu. Chúng là cách chương trình duy trì trạng thái. Các giá trị có hai dạng trong JS: **nguyên thủy** (primitive) và **đối tượng** (object).

Các giá trị được nhúng trong các chương trình sử dụng *literals*:

```js
greeting("My name is Kyle.");
```

Trong chương trình này, giá trị `"My name is Kyle."` là một chuỗi literal nguyên thủy; chuỗi là tập hợp các ký tự được sắp xếp, thường được sử dụng để đại diện cho các từ và câu.

Tôi đã sử dụng ký tự dấu ngoặc kép `"` để *phân định* (bao quanh, tách biệt, xác định) giá trị chuỗi. Nhưng tôi cũng có thể sử dụng ký tự dấu ngoặc đơn `'`. Việc chọn ký tự trích dẫn nào hoàn toàn là phong cách. Điều quan trọng, vì mục đích dễ đọc và bảo trì mã, là chọn một và sử dụng nó nhất quán trong suốt chương trình.

Một tùy chọn khác để phân định một chuỗi literal là sử dụng ký tự back-tick `` ` ``. Tuy nhiên, lựa chọn này không chỉ đơn thuần là phong cách; có một sự khác biệt về hành vi nữa. Hãy xem xét:

```js
console.log("My name is ${ firstName }.");
// My name is ${ firstName }.

console.log('My name is ${ firstName }.');
// My name is ${ firstName }.

console.log(`My name is ${ firstName }.`);
// My name is Kyle.
```

Giả sử chương trình này đã xác định một biến `firstName` với giá trị chuỗi `"Kyle"`, chuỗi được phân định bằng `` ` `` sau đó giải quyết biểu thức biến (được chỉ định bằng `${ .. }`) thành giá trị hiện tại của nó. Điều này được gọi là **nội suy** (interpolation).

Chuỗi được phân định bằng back-tick `` ` `` có thể được sử dụng mà không bao gồm các biểu thức nội suy, nhưng điều đó đánh bại toàn bộ mục đích của cú pháp chuỗi literal thay thế đó:

```js
console.log(
    `Am I confusing you by omitting interpolation?`
);
// Am I confusing you by omitting interpolation?
```

Cách tiếp cận tốt hơn là sử dụng `"` hoặc `'` (một lần nữa, hãy chọn một và tuân theo nó!) cho các chuỗi *trừ khi bạn cần* nội suy; chỉ dành riêng `` ` `` cho các chuỗi sẽ bao gồm các biểu thức nội suy.

Ngoài chuỗi, các chương trình JS thường chứa các giá trị literal nguyên thủy khác như boolean và số:

```js
while (false) {
    console.log(3.141592);
}
```

`while` đại diện cho một loại vòng lặp, một cách để lặp lại các thao tác *trong khi* điều kiện của nó là đúng.

Trong trường hợp này, vòng lặp sẽ không bao giờ chạy (và không có gì được in ra), bởi vì chúng ta đã sử dụng giá trị boolean `false` làm điều kiện vòng lặp. `true` sẽ dẫn đến một vòng lặp tiếp tục mãi mãi, vì vậy hãy cẩn thận!

Số `3.141592`, như bạn có thể biết, là xấp xỉ của số PI toán học đến sáu chữ số đầu tiên. Tuy nhiên, thay vì nhúng một giá trị như vậy, bạn thường sẽ sử dụng giá trị `Math.PI` được xác định trước cho mục đích đó. Một biến thể khác trên các con số là loại nguyên thủy `bigint` (big-integer), được sử dụng để lưu trữ các số lớn tùy ý.

Các con số thường được sử dụng nhất trong các chương trình để đếm các bước, chẳng hạn như lặp lại vòng lặp và truy cập thông tin ở các vị trí số (tức là chỉ mục mảng). Chúng ta sẽ đề cập đến mảng/đối tượng một chút nữa, nhưng ví dụ, nếu có một mảng gọi là `names`, chúng ta có thể truy cập phần tử ở vị trí thứ hai của nó như thế này:

```js
console.log(`My name is ${ names[1] }.`);
// My name is Kyle.
```

Chúng ta đã sử dụng `1` cho phần tử ở vị trí thứ hai, thay vì `2`, bởi vì giống như trong hầu hết các ngôn ngữ lập trình, chỉ mục mảng JS dựa trên 0 (`0` là vị trí đầu tiên).

Ngoài chuỗi, số và boolean, hai giá trị *nguyên thủy* khác trong các chương trình JS là `null` và `undefined`. Mặc dù có sự khác biệt giữa chúng (một số lịch sử và một số đương đại), phần lớn cả hai giá trị đều phục vụ mục đích chỉ ra sự *trống rỗng* (hoặc vắng mặt) của một giá trị.

Nhiều nhà phát triển thích xử lý cả hai một cách nhất quán theo cách này, nghĩa là các giá trị được giả định là không thể phân biệt được. Nếu cẩn thận, điều này thường có thể thực hiện được. Tuy nhiên, an toàn nhất và tốt nhất là chỉ sử dụng `undefined` làm giá trị trống duy nhất, mặc dù `null` có vẻ hấp dẫn ở chỗ nó ngắn hơn để gõ!

```js
while (value != undefined) {
    console.log("Still got something!");
}
```

Giá trị nguyên thủy cuối cùng cần biết là symbol (ký hiệu), là một giá trị có mục đích đặc biệt hoạt động như một giá trị ẩn không thể đoán được. Các symbol hầu như chỉ được sử dụng làm các khóa đặc biệt trên các đối tượng:

```js
hitchhikersGuide[ Symbol("meaning of life") ];
// 42
```

Bạn sẽ không gặp phải việc sử dụng trực tiếp các symbol thường xuyên trong các chương trình JS điển hình. Chúng chủ yếu được sử dụng trong mã cấp thấp như trong các thư viện và framework.

### Mảng và Đối tượng

Bên cạnh các nguyên thủy, loại giá trị khác trong JS là giá trị đối tượng.

Như đã đề cập trước đó, mảng là một loại đối tượng đặc biệt bao gồm một danh sách dữ liệu được sắp xếp và lập chỉ mục bằng số:

```js
var names = [ "Frank", "Kyle", "Peter", "Susan" ];

names.length;
// 4

names[0];
// Frank

names[1];
// Kyle
```

Mảng JS có thể chứa bất kỳ loại giá trị nào, nguyên thủy hoặc đối tượng (bao gồm cả các mảng khác). Như chúng ta sẽ thấy ở cuối Chương 3, ngay cả các hàm cũng là các giá trị có thể được giữ trong mảng hoặc đối tượng.

| LƯU Ý: |
| :--- |
| Các hàm, giống như mảng, là một loại đối tượng đặc biệt (hay còn gọi là kiểu con). Chúng ta sẽ đề cập đến các hàm chi tiết hơn một chút nữa. |

Các đối tượng tổng quát hơn: một tập hợp không có thứ tự, có khóa của bất kỳ giá trị nào khác nhau. Nói cách khác, bạn truy cập phần tử bằng tên vị trí chuỗi (hay còn gọi là "khóa" hoặc "thuộc tính") thay vì bằng vị trí số của nó (như với mảng). Ví dụ:

```js
var me = {
    first: "Kyle",
    last: "Simpson",
    age: 39,
    specialties: [ "JS", "Table Tennis" ]
};

console.log(`My name is ${ me.first }.`);
```

Ở đây, `me` đại diện cho một đối tượng, và `first` đại diện cho tên của một vị trí thông tin trong đối tượng đó (tập hợp giá trị). Một tùy chọn cú pháp khác truy cập thông tin trong một đối tượng bằng thuộc tính/khóa của nó sử dụng dấu ngoặc vuông `[ ]`, chẳng hạn như `me["first"]`.

### Xác định loại giá trị

Để phân biệt các giá trị, toán tử `typeof` cho bạn biết loại tích hợp của nó, nếu là nguyên thủy, hoặc `"object"` nếu không:

```js
typeof 42;                  // "number"
typeof "abc";               // "string"
typeof true;                // "boolean"
typeof undefined;           // "undefined"
typeof null;                // "object" -- ôi, lỗi!
typeof { "a": 1 };          // "object"
typeof [1,2,3];             // "object"
typeof function hello(){};  // "function"
```

| CẢNH BÁO: |
| :--- |
| `typeof null` thật không may trả về `"object"` thay vì `"null"` như mong đợi. Ngoài ra, `typeof` trả về `"function"` cụ thể cho các hàm, nhưng không phải là `"array"` như mong đợi cho các mảng. |

Chuyển đổi từ loại giá trị này sang loại giá trị khác, chẳng hạn như từ chuỗi sang số, được gọi trong JS là "ép kiểu" (coercion). Chúng ta sẽ đề cập đến điều này chi tiết hơn sau trong chương này.

Các giá trị nguyên thủy và giá trị đối tượng hoạt động khác nhau khi chúng được gán hoặc truyền đi. Chúng ta sẽ đề cập đến những chi tiết này trong Phụ lục A, "Giá trị so với Tham chiếu".

## Khai báo và Sử dụng Biến

Để rõ ràng về một điều gì đó có thể không rõ ràng trong phần trước: trong các chương trình JS, các giá trị có thể xuất hiện dưới dạng giá trị literal (như nhiều ví dụ trước minh họa), hoặc chúng có thể được giữ trong các biến; hãy nghĩ về các biến chỉ là các thùng chứa cho các giá trị.

Các biến phải được khai báo (tạo) để được sử dụng. Có nhiều dạng cú pháp khai báo biến (hay còn gọi là "định danh"), và mỗi dạng có các hành vi ngụ ý khác nhau.

Ví dụ, hãy xem xét câu lệnh `var`:

```js
var myName = "Kyle";
var age;
```

Từ khóa `var` khai báo một biến được sử dụng trong phần đó của chương trình, và tùy chọn cho phép gán giá trị ban đầu.

Một từ khóa tương tự khác là `let`:

```js
let myName = "Kyle";
let age;
```

Từ khóa `let` có một số khác biệt so với `var`, với sự khác biệt rõ ràng nhất là `let` cho phép truy cập hạn chế hơn vào biến so với `var`. Điều này được gọi là "phạm vi khối" (block scoping) trái ngược với phạm vi thông thường hoặc phạm vi hàm.

Hãy xem xét:

```js
var adult = true;

if (adult) {
    var myName = "Kyle";
    let age = 39;
    console.log("Shhh, this is a secret!");
}

console.log(myName);
// Kyle

console.log(age);
// Error!
```

Nỗ lực truy cập `age` bên ngoài câu lệnh `if` dẫn đến lỗi, bởi vì `age` được phạm vi khối cho `if`, trong khi `myName` thì không.

Phạm vi khối rất hữu ích để hạn chế mức độ lan rộng của các khai báo biến trong các chương trình của chúng ta, giúp ngăn chặn sự chồng chéo ngẫu nhiên của tên của chúng.

Nhưng `var` vẫn hữu ích ở chỗ nó truyền đạt "biến này sẽ được nhìn thấy bởi một phạm vi rộng hơn (của toàn bộ hàm)". Cả hai hình thức khai báo đều có thể phù hợp trong bất kỳ phần nào của chương trình, tùy thuộc vào hoàn cảnh.

| LƯU Ý: |
| :--- |
| Rất phổ biến khi đề nghị rằng nên tránh `var` để ủng hộ `let` (hoặc `const`!), nói chung là do sự nhầm lẫn nhận thức về cách hành vi phạm vi của `var` đã hoạt động kể từ khi bắt đầu JS. Tôi tin rằng đây là lời khuyên quá hạn chế và cuối cùng là không hữu ích. Nó giả định rằng bạn không thể học và sử dụng một tính năng đúng cách kết hợp với các tính năng khác. Tôi tin rằng bạn *có thể* và *nên* học bất kỳ tính năng nào có sẵn, và sử dụng chúng khi thích hợp! |

Một hình thức khai báo thứ ba là `const`. Nó giống như `let` nhưng có một hạn chế bổ sung là nó phải được cung cấp một giá trị tại thời điểm nó được khai báo, và không thể được gán lại một giá trị khác sau này.

Hãy xem xét:

```js
const myBirthday = true;
let age = 39;

if (myBirthday) {
    age = age + 1;    // OK!
    myBirthday = false;  // Error!
}
```

Hằng số `myBirthday` không được phép gán lại.

Các biến được khai báo `const` không phải là "không thể thay đổi", chúng chỉ không thể được gán lại. Không nên sử dụng `const` với các giá trị đối tượng, bởi vì các giá trị đó vẫn có thể bị thay đổi mặc dù biến không thể được gán lại. Điều này dẫn đến sự nhầm lẫn tiềm ẩn về sau, vì vậy tôi nghĩ rằng thật khôn ngoan khi tránh các tình huống như:

```js
const actors = [
    "Morgan Freeman", "Jennifer Aniston"
];

actors[2] = "Tom Cruise";   // OK :(
actors = [];                // Error!
```

Cách sử dụng ngữ nghĩa tốt nhất của `const` là khi bạn có một giá trị nguyên thủy đơn giản mà bạn muốn đặt tên hữu ích cho nó, chẳng hạn như sử dụng `myBirthday` thay vì `true`. Điều này làm cho các chương trình dễ đọc hơn.

| MẸO: |
| :--- |
| Nếu bạn tuân thủ việc chỉ sử dụng `const` với các giá trị nguyên thủy, bạn tránh được mọi sự nhầm lẫn về việc gán lại (không được phép) so với đột biến (được phép)! Đó là cách an toàn nhất và tốt nhất để sử dụng `const`. |

Ngoài `var` / `let` / `const`, còn có các dạng cú pháp khác khai báo các định danh (biến) trong các phạm vi khác nhau. Ví dụ:

```js
function hello(myName) {
    console.log(`Hello, ${ myName }.`);
}

hello("Kyle");
// Hello, Kyle.
```

Định danh `hello` được tạo trong phạm vi bên ngoài, và nó cũng được liên kết tự động để nó tham chiếu đến hàm. Nhưng tham số được đặt tên `myName` chỉ được tạo bên trong hàm, và do đó chỉ có thể truy cập được bên trong phạm vi của hàm đó. `hello` và `myName` thường hoạt động như được khai báo bằng `var`.

Một cú pháp khác khai báo một biến là mệnh đề `catch`:

```js
try {
    someError();
}
catch (err) {
    console.log(err);
}
```

`err` là một biến phạm vi khối chỉ tồn tại bên trong mệnh đề `catch`, như thể nó đã được khai báo bằng `let`.

## Hàm

Từ "hàm" (function) có nhiều ý nghĩa khác nhau trong lập trình. Ví dụ, trong thế giới Lập trình Hàm (Functional Programming), "hàm" có một định nghĩa toán học chính xác và ngụ ý một tập hợp các quy tắc nghiêm ngặt cần tuân thủ.

Trong JS, chúng ta nên coi "hàm" mang ý nghĩa rộng hơn của một thuật ngữ liên quan khác: "thủ tục" (procedure). Một thủ tục là một tập hợp các câu lệnh có thể được gọi một hoặc nhiều lần, có thể được cung cấp một số đầu vào và có thể trả lại một hoặc nhiều đầu ra.

Từ những ngày đầu của JS, định nghĩa hàm trông giống như:

```js
function awesomeFunction(coolThings) {
    // ..
    return amazingStuff;
}
```

Đây được gọi là khai báo hàm vì nó xuất hiện như một câu lệnh riêng lẻ, không phải là một biểu thức trong một câu lệnh khác. Sự liên kết giữa định danh `awesomeFunction` và giá trị hàm xảy ra trong giai đoạn biên dịch của mã, trước khi mã đó được thực thi.

Ngược lại với câu lệnh khai báo hàm, một biểu thức hàm có thể được định nghĩa và gán như thế này:

```js
// let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function(coolThings) {
    // ..
    return amazingStuff;
};
```

Hàm này là một biểu thức được gán cho biến `awesomeFunction`. Khác với dạng khai báo hàm, một biểu thức hàm không được liên kết với định danh của nó cho đến khi câu lệnh đó chạy (runtime).

Điều cực kỳ quan trọng cần lưu ý là trong JS, các hàm là các giá trị có thể được gán (như được hiển thị trong đoạn mã này) và được truyền đi. Trên thực tế, các hàm JS là một loại đặc biệt của loại giá trị đối tượng. Không phải tất cả các ngôn ngữ đều coi các hàm là giá trị, nhưng điều cần thiết là một ngôn ngữ phải hỗ trợ mẫu lập trình hàm, như JS làm.

Các hàm JS có thể nhận đầu vào tham số:

```js
function greeting(myName) {
    console.log(`Hello, ${ myName }!`);
}

greeting("Kyle");   // Hello, Kyle!
```

Trong đoạn mã này, `myName` được gọi là tham số, hoạt động như một biến cục bộ bên trong hàm. Các hàm có thể được định nghĩa để nhận bất kỳ số lượng tham số nào, từ không có gì trở lên, tùy theo ý bạn. Mỗi tham số được gán giá trị đối số mà bạn truyền vào vị trí đó (`"Kyle"`, ở đây) của cuộc gọi.

Các hàm cũng có thể trả về các giá trị bằng cách sử dụng từ khóa `return`:

```js
function greeting(myName) {
    return `Hello, ${ myName }!`;
}

var msg = greeting("Kyle");

console.log(msg);   // Hello, Kyle!
```

Bạn chỉ có thể `return` một giá trị duy nhất, nhưng nếu bạn có nhiều giá trị để trả về, bạn có thể gói chúng vào một đối tượng/mảng duy nhất.

Vì các hàm là giá trị, chúng có thể được gán làm thuộc tính trên các đối tượng:

```js
var whatToSay = {
    greeting() {
        console.log("Hello!");
    },
    question() {
        console.log("What's your name?");
    },
    answer() {
        console.log("My name is Kyle.");
    }
};

whatToSay.greeting();
// Hello!
```

Trong đoạn mã này, các tham chiếu đến ba hàm (`greeting()`, `question()`, và `answer()`) được bao gồm trong đối tượng được giữ bởi `whatToSay`. Mỗi hàm có thể được gọi bằng cách truy cập thuộc tính để lấy giá trị tham chiếu hàm. So sánh kiểu định nghĩa hàm đơn giản này trên một đối tượng với cú pháp `class` phức tạp hơn được thảo luận sau trong chương này.

Có nhiều dạng khác nhau mà `function` có trong JS. Chúng ta đào sâu vào các biến thể này trong Phụ lục A, "Rất nhiều dạng hàm".

## So sánh

Việc đưa ra quyết định trong các chương trình đòi hỏi phải so sánh các giá trị để xác định danh tính và mối quan hệ của chúng với nhau. JS có một số cơ chế để cho phép so sánh giá trị, vì vậy hãy xem xét kỹ hơn về chúng.

### Bằng nhau... đại loại thế

So sánh phổ biến nhất trong các chương trình JS đặt câu hỏi, "Giá trị X này có *giống như* giá trị Y kia không?" Tuy nhiên, chính xác thì "giống như" thực sự có ý nghĩa gì đối với JS?

Vì lý do công thái học và lịch sử, ý nghĩa phức tạp hơn so với kiểu khớp *danh tính chính xác* rõ ràng. Đôi khi so sánh bằng nhau có ý định khớp *chính xác*, nhưng những lần khác so sánh mong muốn rộng hơn một chút, cho phép khớp *gần giống* hoặc *có thể thay thế*. Nói cách khác, chúng ta phải nhận thức được những khác biệt sắc thái giữa so sánh **bằng nhau** (equality) và so sánh **tương đương** (equivalence).

Nếu bạn đã dành bất kỳ thời gian nào để làm việc với và đọc về JS, bạn chắc chắn đã thấy cái gọi là toán tử "ba dấu bằng" `===`, còn được mô tả là toán tử "bằng nghiêm ngặt". Điều đó có vẻ khá đơn giản, phải không? Chắc chắn, "nghiêm ngặt" có nghĩa là nghiêm ngặt, như trong hẹp và *chính xác*.

Không *chính xác* là vậy.

Vâng, hầu hết các giá trị tham gia so sánh bằng nhau `===` sẽ phù hợp với trực giác *giống hệt nhau* đó. Hãy xem xét một số ví dụ:

```js
3 === 3.0;              // true
"yes" === "yes";        // true
null === null;          // true
false === false;        // true

42 === "42";            // false
"hello" === "Hello";    // false
true === 1;             // false
0 === null;             // false
"" === null;            // false
null === undefined;     // false
```

| LƯU Ý: |
| :--- |
| Một cách khác mà so sánh bằng nhau của `===` thường được mô tả là, "kiểm tra cả giá trị và kiểu". Trong một số ví dụ chúng ta đã xem xét cho đến nay, như `42 === "42"`, *kiểu* của cả hai giá trị (số, chuỗi, v.v.) dường như là yếu tố phân biệt. Tuy nhiên, còn nhiều điều hơn thế nữa. **Tất cả** các so sánh giá trị trong JS đều xem xét kiểu của các giá trị được so sánh, không *chỉ* toán tử `===`. Cụ thể, `===` không cho phép bất kỳ loại chuyển đổi kiểu nào (hay còn gọi là "ép kiểu") trong so sánh của nó, trong khi các so sánh JS khác *có* cho phép ép kiểu. |

Nhưng toán tử `===` có một số sắc thái đối với nó, một thực tế mà nhiều nhà phát triển JS bỏ qua, gây bất lợi cho họ. Toán tử `===` được thiết kế để *nói dối* trong hai trường hợp giá trị đặc biệt: `NaN` và `-0`. Hãy xem xét:

```js
NaN === NaN;            // false
0 === -0;               // true
```

Trong trường hợp của `NaN`, toán tử `===` *nói dối* và nói rằng một lần xuất hiện của `NaN` không bằng một `NaN` khác. Trong trường hợp của `-0` (vâng, đây là một giá trị thực, riêng biệt mà bạn có thể sử dụng có chủ đích trong các chương trình của mình!), toán tử `===` *nói dối* và nói rằng nó bằng giá trị `0` thông thường.

Vì việc *nói dối* về những so sánh như vậy có thể gây phiền toái, tốt nhất là tránh sử dụng `===` cho chúng. Đối với so sánh `NaN`, hãy sử dụng tiện ích `Number.isNaN(..)`, tiện ích này không *nói dối*. Đối với so sánh `-0`, hãy sử dụng tiện ích `Object.is(..)`, tiện ích này cũng không *nói dối*. `Object.is(..)` cũng có thể được sử dụng cho các kiểm tra `NaN` không *nói dối*, nếu bạn thích. Một cách hài hước, bạn có thể nghĩ về `Object.is(..)` như là "bốn dấu bằng" `====`, so sánh thực sự-thực sự-nghiêm ngặt!

Có những lý do lịch sử và kỹ thuật sâu xa hơn cho những lời *nói dối* này, nhưng điều đó không thay đổi thực tế là `===` thực sự không phải là so sánh *bằng nhau nghiêm ngặt chính xác*, theo nghĩa *nghiêm ngặt nhất*.

Câu chuyện trở nên phức tạp hơn nữa khi chúng ta xem xét so sánh các giá trị đối tượng (không phải nguyên thủy). Hãy xem xét:

```js
[ 1, 2, 3 ] === [ 1, 2, 3 ];    // false
{ a: 42 } === { a: 42 }         // false
(x => x * 2) === (x => x * 2)   // false
```

Chuyện gì đang xảy ra ở đây?

Có vẻ hợp lý khi cho rằng kiểm tra bằng nhau xem xét *bản chất* hoặc *nội dung* của giá trị; rốt cuộc, `42 === 42` xem xét giá trị `42` thực tế và so sánh nó. Nhưng khi nói đến các đối tượng, so sánh nhận biết nội dung thường được gọi là "bằng nhau về cấu trúc".

JS không định nghĩa `===` là *bằng nhau về cấu trúc* cho các giá trị đối tượng. Thay vào đó, `===` sử dụng *bằng nhau về danh tính* cho các giá trị đối tượng.

Trong JS, tất cả các giá trị đối tượng được giữ bằng tham chiếu (xem "Giá trị so với Tham chiếu" trong Phụ lục A), được gán và truyền bằng sao chép tham chiếu, **và** đối với cuộc thảo luận hiện tại của chúng ta, được so sánh bằng bằng nhau tham chiếu (danh tính). Hãy xem xét:

```js
var x = [ 1, 2, 3 ];

// gán là sao chép tham chiếu, vì vậy
// y tham chiếu đến *cùng* một mảng như x,
// không phải là một bản sao khác của nó.
var y = x;

y === x;              // true
y === [ 1, 2, 3 ];    // false
x === [ 1, 2, 3 ];    // false
```

Trong đoạn mã này, `y === x` là đúng vì cả hai biến đều giữ tham chiếu đến cùng một mảng ban đầu. Nhưng các so sánh `=== [1,2,3]` đều thất bại vì `y` và `x`, tương ứng, đang được so sánh với các mảng *khác nhau* mới `[1,2,3]`. Cấu trúc và nội dung mảng không quan trọng trong so sánh này, chỉ có **danh tính tham chiếu**.

JS không cung cấp cơ chế so sánh bằng nhau về cấu trúc của các giá trị đối tượng, chỉ so sánh danh tính tham chiếu. Để thực hiện so sánh bằng nhau về cấu trúc, bạn sẽ cần tự thực hiện các kiểm tra.

Nhưng hãy cẩn thận, nó phức tạp hơn bạn nghĩ. Ví dụ, làm thế nào bạn có thể xác định xem hai tham chiếu hàm có "tương đương về cấu trúc" hay không? Ngay cả việc stringifying để so sánh văn bản mã nguồn của chúng cũng sẽ không tính đến những thứ như closure. JS không cung cấp so sánh bằng nhau về cấu trúc vì gần như không thể xử lý tất cả các trường hợp góc cạnh!

### So sánh ép kiểu

Ép kiểu (coercion) có nghĩa là một giá trị của một kiểu được chuyển đổi sang biểu diễn tương ứng của nó trong một kiểu khác (đến bất kỳ mức độ nào có thể). Như chúng ta sẽ thảo luận trong Chương 4, ép kiểu là một trụ cột cốt lõi của ngôn ngữ JS, không phải là một tính năng tùy chọn có thể tránh được một cách hợp lý.

Nhưng khi ép kiểu gặp các toán tử so sánh (như bằng nhau), sự nhầm lẫn và thất vọng không may lại nảy sinh thường xuyên hơn không.

Rất ít tính năng JS thu hút nhiều sự phẫn nộ trong cộng đồng JS rộng lớn hơn toán tử `==`, thường được gọi là toán tử "bằng lỏng lẻo" (loose equality). Phần lớn tất cả các bài viết và diễn ngôn công khai về JS đều lên án toán tử này là được thiết kế kém và nguy hiểm/đầy lỗi khi được sử dụng trong các chương trình JS. Ngay cả người tạo ra ngôn ngữ, Brendan Eich, cũng đã than thở về việc nó được thiết kế như một sai lầm lớn như thế nào.

Theo những gì tôi có thể nói, hầu hết sự thất vọng này đến từ một danh sách khá ngắn các trường hợp góc cạnh khó hiểu, nhưng một vấn đề sâu xa hơn là quan niệm sai lầm cực kỳ phổ biến rằng nó thực hiện các so sánh của nó mà không xem xét các kiểu của các giá trị được so sánh của nó.

Toán tử `==` thực hiện so sánh bằng nhau tương tự như cách `===` thực hiện nó. Trên thực tế, cả hai toán tử đều xem xét kiểu của các giá trị được so sánh. Và nếu so sánh là giữa cùng một kiểu giá trị, cả `==` và `===` **làm chính xác điều tương tự, không có sự khác biệt nào.**

Nếu các kiểu giá trị được so sánh là khác nhau, `==` khác với `===` ở chỗ nó cho phép ép kiểu trước khi so sánh. Nói cách khác, cả hai đều muốn so sánh các giá trị của các kiểu giống nhau, nhưng `==` cho phép chuyển đổi kiểu *trước*, và một khi các kiểu đã được chuyển đổi để giống nhau ở cả hai bên, thì `==` thực hiện điều tương tự như `===`. Thay vì "bằng lỏng lẻo", toán tử `==` nên được mô tả là "bằng ép kiểu" (coercive equality).

Hãy xem xét:

```js
42 == "42";             // true
1 == true;              // true
```

Trong cả hai so sánh, các kiểu giá trị là khác nhau, vì vậy `==` khiến các giá trị không phải số (`"42"` và `true`) được chuyển đổi thành số (`42` và `1`, tương ứng) trước khi so sánh được thực hiện.

Chỉ cần nhận thức được bản chất này của `==`—rằng nó thích so sánh số nguyên thủy hơn—giúp bạn tránh được hầu hết các trường hợp góc cạnh rắc rối, chẳng hạn như tránh xa các cạm bẫy như `"" == 0` hoặc `0 == false`.

Bạn có thể đang nghĩ, "Ồ, chà, tôi sẽ luôn tránh mọi so sánh bằng ép kiểu (sử dụng `===` thay thế) để tránh những trường hợp góc cạnh đó"! Eh, xin lỗi, điều đó không hoàn toàn có khả năng như bạn hy vọng.

Có một cơ hội khá tốt là bạn sẽ sử dụng các toán tử so sánh quan hệ như `<`, `>` (và thậm chí `<=` và `>=`).

Giống như `==`, các toán tử này sẽ hoạt động như thể chúng "nghiêm ngặt" nếu các kiểu được so sánh quan hệ đã khớp, nhưng chúng sẽ cho phép ép kiểu trước (thường là thành số) nếu các kiểu khác nhau.

Hãy xem xét:

```js
var arr = [ "1", "10", "100", "1000" ];
for (let i = 0; i < arr.length && arr[i] < 500; i++) {
    // sẽ chạy 3 lần
}
```

So sánh `i < arr.length` là "an toàn" khỏi ép kiểu vì `i` và `arr.length` luôn là số. Tuy nhiên, `arr[i] < 500` gọi ép kiểu, bởi vì các giá trị `arr[i]` đều là chuỗi. Do đó, các so sánh đó trở thành `1 < 500`, `10 < 500`, `100 < 500`, và `1000 < 500`. Vì cái thứ tư là sai, vòng lặp dừng lại sau lần lặp thứ ba của nó.

Các toán tử quan hệ này thường sử dụng so sánh số, ngoại trừ trường hợp **cả hai** giá trị được so sánh đều đã là chuỗi; trong trường hợp này, chúng sử dụng so sánh theo bảng chữ cái (giống như từ điển) của các chuỗi:

```js
var x = "10";
var y = "9";

x < y;      // true, coi chừng!
```

Không có cách nào để khiến các toán tử quan hệ này tránh ép kiểu, ngoài việc chỉ không bao giờ sử dụng các kiểu không khớp trong các so sánh. Đó có lẽ là một mục tiêu đáng ngưỡng mộ, nhưng vẫn khá có khả năng bạn sẽ gặp phải trường hợp các kiểu *có thể* khác nhau.

Cách tiếp cận khôn ngoan hơn không phải là tránh so sánh ép kiểu, mà là nắm lấy và tìm hiểu những điều cơ bản của chúng.

So sánh ép kiểu xuất hiện ở những nơi khác trong JS, chẳng hạn như các điều kiện (`if`, v.v.), mà chúng ta sẽ xem lại trong Phụ lục A, "So sánh Điều kiện Ép kiểu".

## Cách chúng ta tổ chức trong JS

Hai mẫu chính để tổ chức mã (dữ liệu và hành vi) được sử dụng rộng rãi trên hệ sinh thái JS: lớp (classes) và mô-đun (modules). Các mẫu này không loại trừ lẫn nhau; nhiều chương trình có thể và thực sự sử dụng cả hai. Các chương trình khác sẽ chỉ gắn bó với một mẫu, hoặc thậm chí không mẫu nào cả!

Ở một số khía cạnh, các mẫu này rất khác nhau. Nhưng thú vị thay, theo những cách khác, chúng chỉ là những mặt khác nhau của cùng một đồng xu. Thành thạo JS đòi hỏi phải hiểu cả hai mẫu và nơi chúng thích hợp (và không!).

### Lớp (Classes)

Các thuật ngữ "hướng đối tượng", "hướng lớp" và "lớp" đều chứa đầy chi tiết và sắc thái; chúng không phổ quát trong định nghĩa.

Chúng ta sẽ sử dụng một định nghĩa chung và hơi truyền thống ở đây, định nghĩa có khả năng quen thuộc nhất với những người có nền tảng về các ngôn ngữ "hướng đối tượng" như C++ và Java.

Một lớp trong một chương trình là một định nghĩa về một "loại" cấu trúc dữ liệu tùy chỉnh bao gồm cả dữ liệu và các hành vi hoạt động trên dữ liệu đó. Các lớp xác định cách một cấu trúc dữ liệu như vậy hoạt động, nhưng bản thân các lớp không phải là các giá trị cụ thể. Để có được một giá trị cụ thể mà bạn có thể sử dụng trong chương trình, một lớp phải được *khởi tạo* (với từ khóa `new`) một hoặc nhiều lần.

Hãy xem xét:

```js
class Page {
    constructor(text) {
        this.text = text;
    }

    print() {
        console.log(this.text);
    }
}

class Notebook {
    constructor() {
        this.pages = [];
    }

    addPage(text) {
        var page = new Page(text);
        this.pages.push(page);
    }

    print() {
        for (let page of this.pages) {
            page.print();
        }
    }
}

var mathNotes = new Notebook();
mathNotes.addPage("Arithmetic: + - * / ...");
mathNotes.addPage("Trigonometry: sin cos tan ...");

mathNotes.print();
// ..
```

Trong lớp `Page`, dữ liệu là một chuỗi văn bản được lưu trữ trong thuộc tính thành viên `this.text`. Hành vi là `print()`, một phương thức đổ văn bản ra console.

Đối với lớp `Notebook`, dữ liệu là một mảng các thể hiện `Page`. Hành vi là `addPage(..)`, một phương thức khởi tạo các trang `Page` mới và thêm chúng vào danh sách, cũng như `print()` (in ra tất cả các trang trong sổ tay).

Câu lệnh `mathNotes = new Notebook()` tạo ra một thể hiện của lớp `Notebook`, và `page = new Page(text)` là nơi các thể hiện của lớp `Page` được tạo ra.

Hành vi (phương thức) chỉ có thể được gọi trên các thể hiện (không phải chính các lớp), chẳng hạn như `mathNotes.addPage(..)` và `page.print()`.

Cơ chế `class` cho phép đóng gói dữ liệu (`text` và `pages`) để được tổ chức cùng với các hành vi của chúng (ví dụ: `addPage(..)` và `print()`). Cùng một chương trình có thể được xây dựng mà không cần bất kỳ định nghĩa `class` nào, nhưng nó có thể sẽ ít tổ chức hơn nhiều, khó đọc và suy luận hơn, và dễ bị lỗi và bảo trì kém hơn.

#### Kế thừa Lớp

Một khía cạnh khác vốn có của thiết kế "hướng lớp" truyền thống, mặc dù ít được sử dụng phổ biến hơn trong JS, là "kế thừa" (và "đa hình"). Hãy xem xét:

```js
class Publication {
    constructor(title,author,pubDate) {
        this.title = title;
        this.author = author;
        this.pubDate = pubDate;
    }

    print() {
        console.log(`
            Title: ${ this.title }
            By: ${ this.author }
            ${ this.pubDate }
        `);
    }
}
```

Lớp `Publication` này định nghĩa một tập hợp hành vi chung mà bất kỳ ấn phẩm nào cũng có thể cần.

Bây giờ hãy xem xét các loại ấn phẩm cụ thể hơn, như `Book` và `BlogPost`:

```js
class Book extends Publication {
    constructor(bookDetails) {
        super(
            bookDetails.title,
            bookDetails.author,
            bookDetails.pubDate
        );
        this.publisher = bookDetails.publisher;
        this.ISBN = bookDetails.ISBN;
    }

    print() {
        super.print();
        console.log(`
            Publisher: ${ this.publisher }
            ISBN: ${ this.ISBN }
        `);
    }
}

class BlogPost extends Publication {
    constructor(title,author,pubDate,URL) {
        super(title,author,pubDate);
        this.URL = URL;
    }

    print() {
        super.print();
        console.log(this.URL);
    }
}
```

Cả `Book` và `BlogPost` đều sử dụng mệnh đề `extends` để *mở rộng* định nghĩa chung của `Publication` để bao gồm hành vi bổ sung. Cuộc gọi `super(..)` trong mỗi hàm tạo ủy quyền cho hàm tạo của lớp cha `Publication` cho công việc khởi tạo của nó, và sau đó chúng thực hiện những việc cụ thể hơn theo loại ấn phẩm tương ứng của chúng (hay còn gọi là "lớp con" hoặc "lớp trẻ").

Bây giờ hãy xem xét việc sử dụng các lớp con này:

```js
var YDKJS = new Book({
    title: "You Don't Know JS",
    author: "Kyle Simpson",
    pubDate: "June 2014",
    publisher: "O'Reilly",
    ISBN: "123456-789"
});

YDKJS.print();
// Title: You Don't Know JS
// By: Kyle Simpson
// June 2014
// Publisher: O'Reilly
// ISBN: 123456-789

var forAgainstLet = new BlogPost(
    "For and against let",
    "Kyle Simpson",
    "October 27, 2014",
    "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```

Lưu ý rằng cả hai thể hiện lớp con đều có phương thức `print()`, đây là một sự ghi đè của phương thức `print()` *được kế thừa* từ lớp cha `Publication`. Mỗi phương thức `print()` của lớp con bị ghi đè đó gọi `super.print()` để gọi phiên bản được kế thừa của phương thức `print()`.

Thực tế là cả hai phương thức được kế thừa và bị ghi đè đều có thể có cùng tên và cùng tồn tại được gọi là *đa hình*.

Kế thừa là một công cụ mạnh mẽ để tổ chức dữ liệu/hành vi trong các đơn vị logic riêng biệt (các lớp), nhưng cho phép lớp con hợp tác với lớp cha bằng cách truy cập/sử dụng hành vi và dữ liệu của nó.

### Modules

The module pattern has essentially the same goal as the class pattern, which is to group data and behavior together into logical units. Also like classes, modules can "include" or "access" the data and behaviors of other modules, for cooperation's sake.

But modules have some important differences from classes. Most notably, the syntax is entirely different.
Giống như `==`, các toán tử này sẽ hoạt động như thể chúng "nghiêm ngặt" nếu các kiểu được so sánh quan hệ đã khớp, nhưng chúng sẽ cho phép ép kiểu trước (thường là thành số) nếu các kiểu khác nhau.

Hãy xem xét:

```js
var arr = [ "1", "10", "100", "1000" ];
for (let i = 0; i < arr.length && arr[i] < 500; i++) {
    // sẽ chạy 3 lần
}
```

So sánh `i < arr.length` là "an toàn" khỏi ép kiểu vì `i` và `arr.length` luôn là số. Tuy nhiên, `arr[i] < 500` gọi ép kiểu, bởi vì các giá trị `arr[i]` đều là chuỗi. Do đó, các so sánh đó trở thành `1 < 500`, `10 < 500`, `100 < 500`, và `1000 < 500`. Vì cái thứ tư là sai, vòng lặp dừng lại sau lần lặp thứ ba của nó.

Các toán tử quan hệ này thường sử dụng so sánh số, ngoại trừ trường hợp **cả hai** giá trị được so sánh đều đã là chuỗi; trong trường hợp này, chúng sử dụng so sánh theo bảng chữ cái (giống như từ điển) của các chuỗi:

```js
var x = "10";
var y = "9";

x < y;      // true, coi chừng!
```

Không có cách nào để khiến các toán tử quan hệ này tránh ép kiểu, ngoài việc chỉ không bao giờ sử dụng các kiểu không khớp trong các so sánh. Đó có lẽ là một mục tiêu đáng ngưỡng mộ, nhưng vẫn khá có khả năng bạn sẽ gặp phải trường hợp các kiểu *có thể* khác nhau.

Cách tiếp cận khôn ngoan hơn không phải là tránh so sánh ép kiểu, mà là nắm lấy và tìm hiểu những điều cơ bản của chúng.

So sánh ép kiểu xuất hiện ở những nơi khác trong JS, chẳng hạn như các điều kiện (`if`, v.v.), mà chúng ta sẽ xem lại trong Phụ lục A, "So sánh Điều kiện Ép kiểu".

## Cách chúng ta tổ chức trong JS

Hai mẫu chính để tổ chức mã (dữ liệu và hành vi) được sử dụng rộng rãi trên hệ sinh thái JS: lớp (classes) và mô-đun (modules). Các mẫu này không loại trừ lẫn nhau; nhiều chương trình có thể và thực sự sử dụng cả hai. Các chương trình khác sẽ chỉ gắn bó với một mẫu, hoặc thậm chí không mẫu nào cả!

Ở một số khía cạnh, các mẫu này rất khác nhau. Nhưng thú vị thay, theo những cách khác, chúng chỉ là những mặt khác nhau của cùng một đồng xu. Thành thạo JS đòi hỏi phải hiểu cả hai mẫu và nơi chúng thích hợp (và không!).

### Lớp (Classes)

Các thuật ngữ "hướng đối tượng", "hướng lớp" và "lớp" đều chứa đầy chi tiết và sắc thái; chúng không phổ biến trong định nghĩa.

Chúng ta sẽ sử dụng một định nghĩa chung và hơi truyền thống ở đây, định nghĩa có khả năng quen thuộc nhất với những người có nền tảng về các ngôn ngữ "hướng đối tượng" như C++ và Java.

Một lớp trong một chương trình là một định nghĩa về một "loại" cấu trúc dữ liệu tùy chỉnh bao gồm cả dữ liệu và các hành vi hoạt động trên dữ liệu đó. Các lớp xác định cách một cấu trúc dữ liệu như vậy hoạt động, nhưng bản thân các lớp không phải là các giá trị cụ thể. Để có được một giá trị cụ thể mà bạn có thể sử dụng trong chương trình, một lớp phải được *khởi tạo* (với từ khóa `new`) một hoặc nhiều lần.

Hãy xem xét:

```js
class Page {
    constructor(text) {
        this.text = text;
    }

    print() {
        console.log(this.text);
    }
}

class Notebook {
    constructor() {
        this.pages = [];
    }

    addPage(text) {
        var page = new Page(text);
        this.pages.push(page);
    }

    print() {
        for (let page of this.pages) {
            page.print();
        }
    }
}

var mathNotes = new Notebook();
mathNotes.addPage("Arithmetic: + - * / ...");
mathNotes.addPage("Trigonometry: sin cos tan ...");

mathNotes.print();
// ..
```

Trong lớp `Page`, dữ liệu là một chuỗi văn bản được lưu trữ trong thuộc tính thành viên `this.text`. Hành vi là `print()`, một phương thức đổ văn bản ra console.

Đối với lớp `Notebook`, dữ liệu là một mảng các thể hiện `Page`. Hành vi là `addPage(..)`, một phương thức khởi tạo các trang `Page` mới và thêm chúng vào danh sách, cũng như `print()` (in ra tất cả các trang trong sổ tay).

Câu lệnh `mathNotes = new Notebook()` tạo ra một thể hiện của lớp `Notebook`, và `page = new Page(text)` là nơi các thể hiện của lớp `Page` được tạo ra.

Hành vi (phương thức) chỉ có thể được gọi trên các thể hiện (không phải chính các lớp), chẳng hạn như `mathNotes.addPage(..)` và `page.print()`.

Cơ chế `class` cho phép đóng gói dữ liệu (`text` và `pages`) để được tổ chức cùng với các hành vi của chúng (ví dụ: `addPage(..)` và `print()`). Cùng một chương trình có thể được xây dựng mà không cần bất kỳ định nghĩa `class` nào, nhưng nó có thể sẽ ít tổ chức hơn nhiều, khó đọc và suy luận hơn, và dễ bị lỗi và bảo trì kém hơn.

#### Kế thừa Lớp

Một khía cạnh khác vốn có của thiết kế "hướng lớp" truyền thống, mặc dù ít được sử dụng phổ biến hơn trong JS, là "kế thừa" (và "đa hình"). Hãy xem xét:

```js
class Publication {
    constructor(title,author,pubDate) {
        this.title = title;
        this.author = author;
        this.pubDate = pubDate;
    }

    print() {
        console.log(`
            Title: ${ this.title }
            By: ${ this.author }
            ${ this.pubDate }
        `);
    }
}
```

Lớp `Publication` này định nghĩa một tập hợp hành vi chung mà bất kỳ ấn phẩm nào cũng có thể cần.

Bây giờ hãy xem xét các loại ấn phẩm cụ thể hơn, như `Book` và `BlogPost`:

```js
class Book extends Publication {
    constructor(bookDetails) {
        super(
            bookDetails.title,
            bookDetails.author,
            bookDetails.pubDate
        );
        this.publisher = bookDetails.publisher;
        this.ISBN = bookDetails.ISBN;
    }

    print() {
        super.print();
        console.log(`
            Publisher: ${ this.publisher }
            ISBN: ${ this.ISBN }
        `);
    }
}

class BlogPost extends Publication {
    constructor(title,author,pubDate,URL) {
        super(title,author,pubDate);
        this.URL = URL;
    }

    print() {
        super.print();
        console.log(this.URL);
    }
}
```

Cả `Book` và `BlogPost` đều sử dụng mệnh đề `extends` để *mở rộng* định nghĩa chung của `Publication` để bao gồm hành vi bổ sung. Cuộc gọi `super(..)` trong mỗi hàm tạo ủy quyền cho hàm tạo của lớp cha `Publication` cho công việc khởi tạo của nó, và sau đó chúng thực hiện những việc cụ thể hơn theo loại ấn phẩm tương ứng của chúng (hay còn gọi là "lớp con" hoặc "lớp trẻ").

Bây giờ hãy xem xét việc sử dụng các lớp con này:

```js
var YDKJS = new Book({
    title: "You Don't Know JS",
    author: "Kyle Simpson",
    pubDate: "June 2014",
    publisher: "O'Reilly",
    ISBN: "123456-789"
});

YDKJS.print();
// Title: You Don't Know JS
// By: Kyle Simpson
// June 2014
// Publisher: O'Reilly
// ISBN: 123456-789

var forAgainstLet = new BlogPost(
    "For and against let",
    "Kyle Simpson",
    "October 27, 2014",
    "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```

Lưu ý rằng cả hai thể hiện lớp con đều có phương thức `print()`, đây là một sự ghi đè của phương thức `print()` *được kế thừa* từ lớp cha `Publication`. Mỗi phương thức `print()` của lớp con bị ghi đè đó gọi `super.print()` để gọi phiên bản được kế thừa của phương thức `print()`.

Thực tế là cả hai phương thức được kế thừa và bị ghi đè đều có thể có cùng tên và cùng tồn tại được gọi là *đa hình*.

Kế thừa là một công cụ mạnh mẽ để tổ chức dữ liệu/hành vi trong các đơn vị logic riêng biệt (các lớp), nhưng cho phép lớp con hợp tác với lớp cha bằng cách truy cập/sử dụng hành vi và dữ liệu của nó.

### Mô-đun (Modules)

Mẫu mô-đun về cơ bản có cùng mục tiêu với mẫu lớp: nhóm dữ liệu và hành vi lại với nhau thành các đơn vị logic. Ngoài ra, giống như các lớp, các mô-đun có thể "bao gồm" hoặc truy cập dữ liệu và hành vi của các mô-đun khác, vì mục đích hợp tác.

Nhưng các mô-đun có một số khác biệt quan trọng về cú pháp và hành vi so với các lớp.

#### Cú pháp Mô-đun Cổ điển

Từ những ngày đầu của JS, các mô-đun đã được hỗ trợ như một mẫu sử dụng được xây dựng dựa trên các tính năng JS hiện có, mà không có cú pháp chuyên dụng ("mô-đun cổ điển").

Sự khác biệt chính là các mô-đun cổ điển yêu cầu một hàm bao quanh (thường được gọi là "factory"), hàm này phải được gọi để tạo và trả về một thể hiện của mô-đun.

Hãy xem xét:

```js
function Publication(title,author,pubDate) {
    var publicAPI = {
        print() {
            console.log(`
                Title: ${ title }
                By: ${ author }
                ${ pubDate }
            `);
        }
    };

    return publicAPI;
}

function Book(bookDetails) {
    var pub = Publication(
        bookDetails.title,
        bookDetails.author,
        bookDetails.pubDate
    );

    var publicAPI = {
        print() {
            pub.print();
            console.log(`
                Publisher: ${ bookDetails.publisher }
                ISBN: ${ bookDetails.ISBN }
            `);
        }
    };

    return publicAPI;
}

function BlogPost(title,author,pubDate,URL) {
    var pub = Publication(title,author,pubDate);

    var publicAPI = {
        print() {
            pub.print();
            console.log(URL);
        }
    };

    return publicAPI;
}
```

So sánh các dạng này với các dạng `class` trước đó.

Các hàm `Book(..)` và `BlogPost(..)` cung cấp dữ liệu ban đầu làm tham số, sau đó gọi hàm `Publication(..)` để thiết lập một thể hiện "cha" với dữ liệu chung.

Tiếp theo, một đối tượng `publicAPI` được tạo ra với phương thức `print()` cụ thể cho loại đó. Lưu ý rằng trong mỗi trường hợp, phương thức `print()` tham chiếu đến biến `pub` (thể hiện cha), để gọi phương thức `print()` của nó. Logic này tương tự như cuộc gọi `super.print()` mà chúng ta đã thấy với các lớp.

Cuối cùng, `publicAPI` được trả về từ hàm factory.

Cách sử dụng các mô-đun này:

```js
var YDKJS = Book({
    title: "You Don't Know JS",
    author: "Kyle Simpson",
    pubDate: "June 2014",
    publisher: "O'Reilly",
    ISBN: "123456-789"
});

YDKJS.print();
// Title: You Don't Know JS
// By: Kyle Simpson
// June 2014
// Publisher: O'Reilly
// ISBN: 123456-789

var forAgainstLet = BlogPost(
    "For and against let",
    "Kyle Simpson",
    "October 27, 2014",
    "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```

Sự khác biệt đáng chú ý duy nhất ở đây là chúng ta không sử dụng `new`, gọi các hàm factory mô-đun như các hàm thông thường.

#### Mô-đun ES

Các mô-đun ES (ESM), được giới thiệu cho ngôn ngữ JS trong ES6, nhằm phục vụ cùng một tinh thần như các mô-đun cổ điển, nhưng tính đến các trường hợp sử dụng quan trọng không được hỗ trợ tốt, cụ thể là:

* Có thể sử dụng một mô-đun cho mỗi tệp.
* Mặc định là singleton (chỉ một thể hiện), nhưng có thể hỗ trợ nhiều thể hiện.
* Các API công khai được xuất khẩu tĩnh (để phân tích tĩnh tốt hơn, v.v.).
* Độ phân giải phụ thuộc mô-đun tự động.

Hãy xem xét lại ví dụ `Publication`, `Book`, và `BlogPost` của chúng ta, nhưng sử dụng ESM.

Đầu tiên, `publication.js`:

```js
function printDetails(title,author,pubDate) {
    console.log(`
        Title: ${ title }
        By: ${ author }
        ${ pubDate }
    `);
}

export function create(title,author,pubDate) {
    var publicAPI = {
        print() {
            printDetails(title,author,pubDate);
        }
    };

    return publicAPI;
}
```

Để nhập và sử dụng mô-đun này, `blogpost.js`:

```js
import { create as createPub } from "publication.js";

function printDetails(pub,URL) {
    pub.print();
    console.log(URL);
}

export function create(title,author,pubDate,URL) {
    var pub = createPub(title,author,pubDate);

    var publicAPI = {
        print() {
            printDetails(pub,URL);
        }
    };

    return publicAPI;
}
```

Và cuối cùng, `main.js`:

```js
import { create as createBlogPost } from "blogpost.js";

var forAgainstLet = createBlogPost(
    "For and against let",
    "Kyle Simpson",
    "October 27, 2014",
    "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```

Như bạn có thể thấy, ESM cho phép chúng ta định nghĩa các mô-đun, mỗi mô-đun trong một tệp riêng biệt, và sau đó nhập chúng vào các tệp khác để sử dụng.

Các biến/hàm được định nghĩa ở cấp cao nhất của tệp mô-đun là riêng tư (ẩn) trừ khi chúng được xuất khẩu (sử dụng từ khóa `export`).

Trong ví dụ này, tôi đã chọn xuất khẩu một hàm factory có tên `create(..)`, hàm này khi được gọi sẽ tạo ra một thể hiện của mô-đun đó. Đây là cách tiếp cận thân thiện nhất để duy trì các khả năng nhiều thể hiện tương tự như chúng ta đã thấy với các mô-đun cổ điển.

Tuy nhiên, bạn cũng có thể chọn xuất khẩu một đối tượng đơn giản với các hàm trên đó, nếu bạn chỉ cần một thể hiện singleton duy nhất của mô-đun.

Sự khác biệt đáng chú ý nhất giữa ESM và các mô-đun cổ điển (hoặc các lớp) là bạn phải chia các định nghĩa của mình thành các tệp riêng biệt. Có nhiều điều thú vị hơn về ESM mà chúng ta sẽ khám phá trong suốt phần còn lại của cuốn sách này và bộ sách.

## Tóm tắt

Chương này chỉ mới bắt đầu làm xước bề mặt của các khái niệm chính trong JS.

Chúng ta đã thảo luận về cách JS coi mỗi tệp là chương trình riêng của nó, nhưng chúng có thể hoạt động cùng nhau. Chúng ta đã xem xét các loại giá trị (nguyên thủy so với đối tượng) và cách chuyển đổi giữa chúng (ép kiểu). Chúng ta đã học cách các hàm là các giá trị có thể được truyền đi, và cách chúng giữ các biến trong các phạm vi lồng nhau. Và chúng ta đã thấy các mẫu lớp và mô-đun để tổ chức dữ liệu và hành vi.

Nếu bất kỳ chủ đề nào trong số này vẫn còn mơ hồ hoặc khó hiểu, đừng lo lắng! Chúng ta chỉ mới bắt đầu. Chúng ta sẽ xem lại và khám phá sâu hơn từng chủ đề này trong các chương tiếp theo và trong suốt phần còn lại của bộ sách.

Bạn đã sẵn sàng để đào sâu hơn chưa? Hãy xem chương tiếp theo, "Đào sâu vào Cội rễ của JS".
