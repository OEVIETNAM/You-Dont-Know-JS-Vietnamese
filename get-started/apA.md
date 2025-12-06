---
layout: default
title: Phụ lục A
parent: Bắt đầu
nav_order: 6
---

# You Don't Know JS Yet: Bắt đầu - Ấn bản thứ 2
# Phụ lục A: Khám phá Thêm

Trong phụ lục này, chúng ta sẽ khám phá một số chủ đề từ văn bản chương chính chi tiết hơn một chút. Hãy coi nội dung này như một bản xem trước tùy chọn của một số chi tiết sắc thái hơn được đề cập trong phần còn lại của bộ sách.

## Giá trị so với Tham chiếu

Trong Chương 2, chúng ta đã giới thiệu hai loại giá trị chính: nguyên thủy (primitives) và đối tượng (objects). Nhưng chúng ta chưa thảo luận về một sự khác biệt chính giữa hai loại này: cách các giá trị này được gán và truyền đi.

Trong nhiều ngôn ngữ, nhà phát triển có thể chọn giữa việc gán/truyền một giá trị dưới dạng chính giá trị đó, hoặc dưới dạng tham chiếu đến giá trị. Tuy nhiên, trong JS, quyết định này hoàn toàn được xác định bởi loại giá trị. Điều đó làm ngạc nhiên rất nhiều nhà phát triển từ các ngôn ngữ khác khi họ bắt đầu sử dụng JS.

Nếu bạn gán/truyền chính một giá trị, giá trị đó sẽ được sao chép. Ví dụ:

```js
var myName = "Kyle";

var yourName = myName;
```

Ở đây, biến `yourName` có một bản sao riêng biệt của chuỗi `"Kyle"` từ giá trị được lưu trữ trong `myName`. Đó là bởi vì giá trị là một nguyên thủy, và các giá trị nguyên thủy luôn được gán/truyền dưới dạng **bản sao giá trị**.

Đây là cách bạn có thể chứng minh có hai giá trị riêng biệt liên quan:

```js
var myName = "Kyle";

var yourName = myName;

myName = "Frank";

console.log(myName);
// Frank

console.log(yourName);
// Kyle
```

Thấy cách `yourName` không bị ảnh hưởng bởi việc gán lại `myName` thành `"Frank"` không? Đó là bởi vì mỗi biến giữ bản sao giá trị của riêng nó.

Ngược lại, tham chiếu là ý tưởng rằng hai hoặc nhiều biến đang trỏ đến cùng một giá trị, sao cho việc sửa đổi giá trị được chia sẻ này sẽ được phản ánh bằng quyền truy cập thông qua bất kỳ tham chiếu nào trong số đó. Trong JS, chỉ các giá trị đối tượng (mảng, đối tượng, hàm, v.v.) được coi là tham chiếu.

Hãy xem xét:

```js
var myAddress = {
    street: "123 JS Blvd",
    city: "Austin",
    state: "TX"
};

var yourAddress = myAddress;

// Tôi phải chuyển đến một ngôi nhà mới!
myAddress.street = "456 TS Ave";

console.log(yourAddress.street);
// 456 TS Ave
```

Vì giá trị được gán cho `myAddress` là một đối tượng, nó được giữ/gán bằng tham chiếu, và do đó việc gán cho biến `yourAddress` là một bản sao của tham chiếu, không phải chính giá trị đối tượng. Đó là lý do tại sao giá trị cập nhật được gán cho `myAddress.street` được phản ánh khi chúng ta truy cập `yourAddress.street`. `myAddress` và `yourAddress` có các bản sao của tham chiếu đến đối tượng được chia sẻ duy nhất, vì vậy cập nhật cho một cái là cập nhật cho cả hai.

Một lần nữa, JS chọn hành vi sao chép giá trị so với sao chép tham chiếu dựa trên loại giá trị. Các nguyên thủy được giữ theo giá trị, các đối tượng được giữ theo tham chiếu. Không có cách nào để ghi đè điều này trong JS, theo cả hai hướng.

## Rất Nhiều Dạng Hàm

Nhớ lại đoạn mã này từ phần "Hàm" trong Chương 2:

```js
var awesomeFunction = function(coolThings) {
    // ..
    return amazingStuff;
};
```

Biểu thức hàm ở đây được gọi là *biểu thức hàm ẩn danh* (anonymous function expression), vì nó không có định danh tên giữa từ khóa `function` và danh sách tham số `(..)`. Điểm này gây nhầm lẫn cho nhiều nhà phát triển JS vì kể từ ES6, JS thực hiện "suy luận tên" trên một hàm ẩn danh:

```js
awesomeFunction.name;
// "awesomeFunction"
```

Thuộc tính `name` của một hàm sẽ tiết lộ tên được đặt trực tiếp của nó (trong trường hợp khai báo) hoặc tên được suy luận của nó trong trường hợp biểu thức hàm ẩn danh. Giá trị đó thường được sử dụng bởi các công cụ dành cho nhà phát triển khi kiểm tra một giá trị hàm hoặc khi báo cáo dấu vết ngăn xếp lỗi.

Vì vậy, ngay cả một biểu thức hàm ẩn danh *có thể* nhận được một cái tên. Tuy nhiên, suy luận tên chỉ xảy ra trong các trường hợp hạn chế chẳng hạn như khi biểu thức hàm được gán (với `=`). Nếu bạn truyền một biểu thức hàm làm đối số cho một cuộc gọi hàm, ví dụ, không có suy luận tên nào xảy ra; thuộc tính `name` sẽ là một chuỗi rỗng, và bảng điều khiển dành cho nhà phát triển thường sẽ báo cáo "(anonymous function)".

Ngay cả khi một cái tên được suy luận, **nó vẫn là một hàm ẩn danh.** Tại sao? Bởi vì tên được suy luận là một giá trị chuỗi siêu dữ liệu, không phải là một định danh có sẵn để tham chiếu đến hàm. Một hàm ẩn danh không có định danh để sử dụng để tham chiếu đến chính nó từ bên trong chính nó—cho đệ quy, hủy liên kết sự kiện, v.v.

So sánh dạng biểu thức hàm ẩn danh với:

```js
// let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function someName(coolThings) {
    // ..
    return amazingStuff;
};

awesomeFunction.name;
// "someName"
```

Biểu thức hàm này là một *biểu thức hàm được đặt tên* (named function expression), vì định danh `someName` được liên kết trực tiếp với biểu thức hàm tại thời điểm biên dịch; liên kết với định danh `awesomeFunction` vẫn không xảy ra cho đến thời gian chạy tại thời điểm của câu lệnh đó. Hai định danh đó không cần phải khớp nhau; đôi khi có ý nghĩa khi để chúng khác nhau, những lần khác tốt hơn là để chúng giống nhau.

Cũng lưu ý rằng tên hàm rõ ràng, định danh `someName`, được ưu tiên khi gán một *tên* cho thuộc tính `name`.

Các biểu thức hàm nên được đặt tên hay ẩn danh? Ý kiến khác nhau rất nhiều về điều này. Hầu hết các nhà phát triển có xu hướng không quan tâm đến việc sử dụng các hàm ẩn danh. Chúng ngắn hơn, và chắc chắn phổ biến hơn trong phạm vi rộng lớn của mã JS ngoài kia.

Theo ý kiến của tôi, nếu một hàm tồn tại trong chương trình của bạn, nó có một mục đích; nếu không, hãy loại bỏ nó! Và nếu nó có một mục đích, nó có một cái tên tự nhiên mô tả mục đích đó.

Nếu một hàm có tên, bạn là tác giả mã nên bao gồm tên đó trong mã, để người đọc không phải suy luận tên đó từ việc đọc và thực thi tinh thần mã nguồn của hàm đó. Ngay cả một thân hàm tầm thường như `x * 2` cũng phải được đọc để suy luận một cái tên như "double" hoặc "multBy2"; công việc tinh thần thêm ngắn gọn đó là không cần thiết khi bạn chỉ cần dành một giây để đặt tên cho hàm là "double" hoặc "multBy2" *một lần*, tiết kiệm cho người đọc công việc tinh thần lặp đi lặp lại đó mỗi khi nó được đọc trong tương lai.

Thật đáng tiếc ở một số khía cạnh, có nhiều dạng định nghĩa hàm khác trong JS tính đến đầu năm 2020 (có thể nhiều hơn trong tương lai!).

Dưới đây là một số dạng khai báo khác:

```js
// khai báo hàm generator
function *two() { .. }

// khai báo hàm async
async function three() { .. }

// khai báo hàm async generator
async function *four() { .. }

// khai báo xuất hàm được đặt tên (mô-đun ES6)
export function five() { .. }
```

Và đây là một số dạng biểu thức hàm (nhiều!) khác:

```js
// IIFE
(function(){ .. })();
(function namedIIFE(){ .. })();

// IIFE không đồng bộ
(async function(){ .. })();
(async function namedAIIFE(){ .. })();

// biểu thức hàm mũi tên (arrow function)
var f;
f = () => 42;
f = x => x * 2;
f = (x) => x * 2;
f = (x,y) => x * y;
f = x => ({ x: x * 2 });
f = x => { return x * 2; };
f = async x => {
    var y = await doSomethingAsync(x);
    return y * 2;
};
someOperation( x => x * 2 );
// ..
```

Hãy nhớ rằng các biểu thức hàm mũi tên là **ẩn danh về mặt cú pháp**, có nghĩa là cú pháp không cung cấp cách để cung cấp một định danh tên trực tiếp cho hàm. Biểu thức hàm có thể nhận được một tên được suy luận, nhưng chỉ khi nó là một trong các dạng gán, không phải trong dạng (phổ biến hơn!) được truyền dưới dạng đối số cuộc gọi hàm (như trong dòng cuối cùng của đoạn mã).

Vì tôi không nghĩ rằng các hàm ẩn danh là một ý tưởng hay để sử dụng thường xuyên trong các chương trình của bạn, tôi không phải là người hâm mộ việc sử dụng dạng hàm mũi tên `=>`. Loại hàm này thực sự có một mục đích cụ thể (tức là, xử lý từ khóa `this` theo từ vựng), nhưng điều đó không có nghĩa là chúng ta nên sử dụng nó cho mọi hàm chúng ta viết. Sử dụng công cụ thích hợp nhất cho mỗi công việc.

Các hàm cũng có thể được chỉ định trong các định nghĩa lớp và định nghĩa literal đối tượng. Chúng thường được gọi là "phương thức" khi ở trong các dạng này, mặc dù trong JS thuật ngữ này không có nhiều khác biệt có thể quan sát được so với "hàm":

```js
class SomethingKindaGreat {
    // phương thức lớp
    coolMethod() { .. }   // không có dấu phẩy!
    boringMethod() { .. }
}

var EntirelyDifferent = {
    // phương thức đối tượng
    coolMethod() { .. },   // dấu phẩy!
    boringMethod() { .. },

    // thuộc tính biểu thức hàm (ẩn danh)
    oldSchool: function() { .. }
};
```

Phù! Đó là rất nhiều cách khác nhau để định nghĩa hàm.

Không có con đường tắt đơn giản nào ở đây; bạn chỉ cần xây dựng sự quen thuộc với tất cả các dạng hàm để bạn có thể nhận ra chúng trong mã hiện có và sử dụng chúng một cách thích hợp trong mã bạn viết. Hãy nghiên cứu chúng kỹ lưỡng và thực hành!

## So sánh Có điều kiện Ép buộc

Vâng, tên phần đó khá dài dòng. Nhưng chúng ta đang nói về cái gì? Chúng ta đang nói về các biểu thức điều kiện cần thực hiện các so sánh định hướng ép buộc để đưa ra quyết định của chúng.

Các câu lệnh `if` và `? :`-ba ngôi, cũng như các mệnh đề kiểm tra trong các vòng lặp `while` và `for`, tất cả đều thực hiện một so sánh giá trị ngầm định. Nhưng loại nào? Là "nghiêm ngặt" (strict) hay "ép buộc" (coercive)? Cả hai, thực ra.

Hãy xem xét:

```js
var x = 1;

if (x) {
    // sẽ chạy!
}

while (x) {
    // sẽ chạy, một lần!
    x = false;
}
```

Bạn có thể nghĩ về các biểu thức điều kiện `(x)` này như thế này:

```js
var x = 1;

if (x == true) {
    // sẽ chạy!
}

while (x == true) {
    // sẽ chạy, một lần!
    x = false;
}
```

Trong trường hợp cụ thể này -- giá trị của `x` là `1` -- mô hình tinh thần đó hoạt động, nhưng nó không chính xác rộng hơn. Hãy xem xét:

```js
var x = "hello";

if (x) {
    // sẽ chạy!
}

if (x == true) {
    // sẽ không chạy :(
}
```

Rất tiếc. Vậy câu lệnh `if` thực sự đang làm gì? Đây là mô hình tinh thần chính xác hơn:

```js
var x = "hello";

if (Boolean(x) == true) {
    // sẽ chạy
}

// cũng giống như là:

if (Boolean(x) === true) {
    // sẽ chạy
}
```

Vì hàm `Boolean(..)` luôn trả về một giá trị kiểu boolean, `==` so với `===` trong đoạn mã này là không liên quan; cả hai đều sẽ làm điều tương tự. Nhưng phần quan trọng là thấy rằng trước khi so sánh, một sự ép buộc xảy ra, từ bất kỳ loại nào `x` hiện tại, sang boolean.

Bạn chỉ không thể thoát khỏi sự ép buộc trong các so sánh JS. Hãy bắt tay vào và học chúng.

## Các "Lớp" Nguyên mẫu

Trong Chương 3, chúng ta đã giới thiệu các nguyên mẫu và chỉ ra cách chúng ta có thể liên kết các đối tượng thông qua một chuỗi nguyên mẫu.

Một cách khác để kết nối các liên kết nguyên mẫu như vậy đã đóng vai trò là người tiền nhiệm (thành thật mà nói, xấu xí) cho sự thanh lịch của hệ thống `class` ES6 (xem Chương 2, "Lớp"), và được gọi là các lớp nguyên mẫu.

| MẸO: |
| :--- |
| Mặc dù phong cách mã này khá hiếm gặp trong JS ngày nay, nhưng vẫn còn khá phổ biến một cách khó hiểu khi được hỏi về nó trong các cuộc phỏng vấn xin việc! |

Trước tiên hãy nhớ lại phong cách mã hóa `Object.create(..)`:

```js
var Classroom = {
    welcome() {
        console.log("Welcome, students!");
    }
};

var mathClass = Object.create(Classroom);

mathClass.welcome();
// Welcome, students!
```

Ở đây, một đối tượng `mathClass` được liên kết qua nguyên mẫu của nó với một đối tượng `Classroom`. Thông qua liên kết này, cuộc gọi hàm `mathClass.welcome()` được ủy quyền cho phương thức được định nghĩa trên `Classroom`.

Mẫu lớp nguyên mẫu sẽ dán nhãn hành vi ủy quyền này là "kế thừa", và thay vào đó đã định nghĩa nó (với cùng hành vi) như sau:

```js
function Classroom() {
    // ..
}

Classroom.prototype.welcome = function hello() {
    console.log("Welcome, students!");
};

var mathClass = new Classroom();

mathClass.welcome();
// Welcome, students!
```

Tất cả các hàm theo mặc định đều tham chiếu đến một đối tượng trống tại một thuộc tính có tên `prototype`. Bất chấp việc đặt tên gây nhầm lẫn, đây **không phải** là *nguyên mẫu* của hàm (nơi hàm được liên kết nguyên mẫu đến), mà là đối tượng nguyên mẫu để *liên kết đến* khi các đối tượng khác được tạo bằng cách gọi hàm với `new`.

Chúng ta thêm một thuộc tính `welcome` trên đối tượng trống đó (được gọi là `Classroom.prototype`), trỏ đến hàm `hello()`.

Sau đó `new Classroom()` tạo ra một đối tượng mới (được gán cho `mathClass`), và liên kết nguyên mẫu nó với đối tượng `Classroom.prototype` hiện có.

Mặc dù `mathClass` không có thuộc tính/hàm `welcome()`, nó ủy quyền thành công cho hàm `Classroom.prototype.welcome()`.

Mẫu "lớp nguyên mẫu" này hiện bị phản đối mạnh mẽ, ủng hộ việc sử dụng cơ chế `class` của ES6:

```js
class Classroom {
    constructor() {
        // ..
    }

    welcome() {
        console.log("Welcome, students!");
    }
}

var mathClass = new Classroom();

mathClass.welcome();
// Welcome, students!
```

Dưới vỏ bọc, cùng một liên kết nguyên mẫu được kết nối, nhưng cú pháp `class` này phù hợp với mẫu thiết kế hướng lớp sạch sẽ hơn nhiều so với "các lớp nguyên mẫu".
