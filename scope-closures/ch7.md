---
layout: default
title: Chương 7
parent: Phạm Vi & Closures
nav_order: 8
---

# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Chương 7: Sử Dụng Closures

Đến thời điểm này, chúng ta đã tập trung vào các chi tiết của phạm vi từ vựng, và cách điều đó ảnh hưởng đến việc tổ chức và sử dụng các biến trong các chương trình của chúng ta.

Sự chú ý của chúng ta một lần nữa chuyển sang sự trừu tượng rộng hơn, đến chủ đề lịch sử có phần đáng sợ của closure. Đừng lo lắng! Bạn không cần bằng cấp khoa học máy tính cao cấp để hiểu nó. Mục tiêu rộng lớn của chúng ta trong cuốn sách này không chỉ đơn thuần là hiểu phạm vi, mà còn sử dụng nó hiệu quả hơn trong cấu trúc các chương trình của chúng ta; closure là trung tâm của nỗ lực đó.

Nhớ lại kết luận chính của Chương 6: nguyên tắc *phơi bày tối thiểu* (POLE) khuyến khích chúng ta sử dụng phạm vi khối (và hàm) để hạn chế sự phơi bày phạm vi của các biến. Điều này giúp giữ cho mã dễ hiểu và dễ bảo trì, và giúp tránh nhiều cạm bẫy phạm vi (tức là, xung đột tên, v.v.).

Closure xây dựng trên cách tiếp cận này: đối với các biến chúng ta cần sử dụng theo thời gian, thay vì đặt chúng trong các phạm vi bên ngoài lớn hơn, chúng ta có thể đóng gói (phạm vi hẹp hơn) chúng nhưng vẫn bảo tồn quyền truy cập từ bên trong các hàm, để sử dụng rộng rãi hơn. Các hàm *nhớ* các biến phạm vi được tham chiếu này thông qua closure.

Chúng ta đã thấy một ví dụ về loại closure này trong chương trước (`factorial(..)` trong Chương 6), và bạn gần như chắc chắn đã sử dụng nó trong các chương trình của riêng mình. Nếu bạn đã từng viết một callback truy cập các biến bên ngoài phạm vi riêng của nó... đoán xem!? Đó là closure.

Closure là một trong những đặc điểm ngôn ngữ quan trọng nhất từng được phát minh trong lập trình—nó làm nền tảng cho các mô hình lập trình chính, bao gồm Lập trình Hàm (FP), modules, và thậm chí một chút thiết kế hướng lớp. Làm quen với closure là bắt buộc để làm chủ JS và tận dụng hiệu quả nhiều mẫu thiết kế quan trọng trong suốt mã của bạn.

Giải quyết tất cả các khía cạnh của closure đòi hỏi một núi thảo luận và mã đáng sợ trong suốt chương này. Hãy chắc chắn dành thời gian của bạn và đảm bảo bạn thoải mái với từng chút trước khi chuyển sang phần tiếp theo.

## Nhìn Thấy Closure

Closure ban đầu là một khái niệm toán học, từ phép tính lambda. Nhưng tôi sẽ không liệt kê các công thức toán học hoặc sử dụng một loạt các ký hiệu và biệt ngữ để định nghĩa nó.

Thay vào đó, tôi sẽ tập trung vào một quan điểm thực tế. Chúng ta sẽ bắt đầu bằng cách định nghĩa closure về mặt những gì chúng ta có thể quan sát trong hành vi khác nhau của các chương trình của chúng ta, trái ngược với nếu closure không có mặt trong JS. Tuy nhiên, sau này trong chương này, chúng ta sẽ lật ngược closure để nhìn nó từ một *quan điểm thay thế*.

Closure là một hành vi của các hàm và chỉ các hàm. Nếu bạn không xử lý một hàm, closure không áp dụng. Một đối tượng không thể có closure, cũng như một lớp không có closure (mặc dù các hàm/phương thức của nó có thể). Chỉ các hàm mới có closure.

Để closure được quan sát, một hàm phải được gọi, và cụ thể nó phải được gọi trong một nhánh khác của chuỗi phạm vi so với nơi nó được định nghĩa ban đầu. Một hàm thực thi trong cùng phạm vi nó được định nghĩa sẽ không thể hiện bất kỳ hành vi khác biệt nào có thể quan sát được với hoặc không có closure là có thể; theo quan điểm quan sát và định nghĩa, đó không phải là closure.

Hãy xem một số mã, được chú thích với các màu bong bóng phạm vi liên quan của nó (xem Chương 2):

```js
// phạm vi bên ngoài/toàn cục: RED(1)

function lookupStudent(studentID) {
    // phạm vi hàm: BLUE(2)

    var students = [
        { id: 14, name: "Kyle" },
        { id: 73, name: "Suzy" },
        { id: 112, name: "Frank" },
        { id: 6, name: "Sarah" }
    ];

    return function greetStudent(greeting){
        // phạm vi hàm: GREEN(3)

        var student = students.find(
            student => student.id == studentID
        );

        return `${ greeting }, ${ student.name }!`;
    };
}

var chosenStudents = [
    lookupStudent(6),
    lookupStudent(112)
];

// truy cập tên của hàm:
chosenStudents[0].name;
// greetStudent

chosenStudents[0]("Hello");
// Hello, Sarah!

chosenStudents[1]("Howdy");
// Howdy, Frank!
```

Điều đầu tiên cần chú ý về mã này là hàm bên ngoài `lookupStudent(..)` tạo và trả về một hàm bên trong được gọi là `greetStudent(..)`. `lookupStudent(..)` được gọi hai lần, tạo ra hai thể hiện riêng biệt của hàm `greetStudent(..)` bên trong của nó, cả hai đều được lưu vào mảng `chosenStudents`.

Chúng ta xác minh trường hợp đó bằng cách kiểm tra thuộc tính `.name` của hàm được trả về được lưu trong `chosenStudents[0]`, và nó thực sự là một thể hiện của `greetStudent(..)` bên trong.

Sau khi mỗi cuộc gọi đến `lookupStudent(..)` kết thúc, có vẻ như tất cả các biến bên trong của nó sẽ bị loại bỏ và GC'd (thu gom rác). Hàm bên trong là thứ duy nhất có vẻ được trả về và bảo tồn. Nhưng đây là nơi hành vi khác biệt theo những cách chúng ta có thể bắt đầu quan sát.

Trong khi `greetStudent(..)` nhận một đối số duy nhất làm tham số có tên `greeting`, nó cũng tham chiếu đến cả `students` và `studentID`, các định danh đến từ phạm vi bao quanh của `lookupStudent(..)`. Mỗi tham chiếu đó từ hàm bên trong đến biến trong một phạm vi bên ngoài được gọi là một *closure*. Theo thuật ngữ học thuật, mỗi thể hiện của `greetStudent(..)` *đóng trên* (closes over) các biến bên ngoài `students` và `studentID`.

Vậy các closure đó làm gì ở đây, theo nghĩa cụ thể, có thể quan sát được?

Closure cho phép `greetStudent(..)` tiếp tục truy cập các biến bên ngoài đó ngay cả sau khi phạm vi bên ngoài đã kết thúc (khi mỗi cuộc gọi đến `lookupStudent(..)` hoàn thành). Thay vì các thể hiện của `students` và `studentID` bị GC'd, chúng ở lại trong bộ nhớ. Tại một thời điểm sau đó khi một trong hai thể hiện của hàm `greetStudent(..)` được gọi, các biến đó vẫn ở đó, giữ các giá trị hiện tại của chúng.

Nếu các hàm JS không có closure, việc hoàn thành mỗi cuộc gọi `lookupStudent(..)` sẽ ngay lập tức phá bỏ phạm vi của nó và GC các biến `students` và `studentID`. Khi chúng ta sau đó gọi một trong các hàm `greetStudent(..)`, điều gì sẽ xảy ra sau đó?

Nếu `greetStudent(..)` cố gắng truy cập những gì nó nghĩ là một viên bi BLUE(2), nhưng viên bi đó thực sự không tồn tại (nữa), giả định hợp lý là chúng ta nên nhận được một `ReferenceError`, đúng không?

Nhưng chúng ta không nhận được lỗi. Thực tế là việc thực thi `chosenStudents[0]("Hello")` hoạt động và trả về cho chúng ta thông báo "Hello, Sarah!", có nghĩa là nó vẫn có thể truy cập các biến `students` và `studentID`. Đây là một quan sát trực tiếp về closure!

### Closure Được Chỉ Định

Thực ra, chúng ta đã lướt qua một chi tiết nhỏ trong cuộc thảo luận trước đó mà tôi đoán nhiều độc giả đã bỏ lỡ!

Bởi vì cú pháp cho các hàm mũi tên `=>` quá ngắn gọn, thật dễ quên rằng chúng vẫn tạo ra một phạm vi (như đã khẳng định trong "Hàm Mũi Tên" trong Chương 3). Hàm mũi tên `student => student.id == studentID` đang tạo ra một bong bóng phạm vi khác bên trong phạm vi hàm `greetStudent(..)`.

Xây dựng trên phép ẩn dụ về các xô và bong bóng màu từ Chương 2, nếu chúng ta đang tạo một sơ đồ màu cho mã này, có một phạm vi thứ tư ở mức lồng nhau trong cùng này, vì vậy chúng ta cần một màu thứ tư; có lẽ chúng ta sẽ chọn ORANGE(4) cho phạm vi đó:

```js
var student = students.find(
    student =>
        // phạm vi hàm: ORANGE(4)
        student.id == studentID
);
```

Tham chiếu BLUE(2) `studentID` thực sự nằm bên trong phạm vi ORANGE(4) thay vì phạm vi GREEN(3) của `greetStudent(..)`; ngoài ra, tham số `student` của hàm mũi tên là ORANGE(4), che khuất `student` GREEN(3).

Hậu quả ở đây là hàm mũi tên này được truyền dưới dạng callback cho phương thức `find(..)` của mảng phải giữ closure trên `studentID`, thay vì `greetStudent(..)` giữ closure đó. Đó không phải là vấn đề quá lớn, vì mọi thứ vẫn hoạt động như mong đợi. Chỉ quan trọng là không bỏ qua thực tế rằng ngay cả các hàm mũi tên nhỏ bé cũng có thể tham gia vào bữa tiệc closure.

### Cộng Dồn Closures

Hãy xem xét một trong những ví dụ kinh điển thường được trích dẫn cho closure:

```js
function adder(num1) {
    return function addTo(num2){
        return num1 + num2;
    };
}

var add10To = adder(10);
var add42To = adder(42);

add10To(15);    // 25
add42To(9);     // 51
```

Mỗi thể hiện của hàm `addTo(..)` bên trong đang đóng trên biến `num1` riêng của nó (với các giá trị `10` và `42`, tương ứng), vì vậy những `num1` đó không biến mất chỉ vì `adder(..)` kết thúc. Khi chúng ta sau đó gọi một trong những thể hiện `addTo(..)` bên trong đó, chẳng hạn như cuộc gọi `add10To(15)`, biến `num1` được đóng trên của nó vẫn tồn tại và vẫn giữ giá trị `10` ban đầu. Do đó, hoạt động có thể thực hiện `10 + 15` và trả về câu trả lời `25`.

Một chi tiết quan trọng có thể đã quá dễ dàng để lướt qua trong đoạn trước, vì vậy hãy củng cố nó: closure được liên kết với một thể hiện của một hàm, thay vì định nghĩa từ vựng duy nhất của nó. Trong đoạn trích trước, chỉ có một hàm `addTo(..)` bên trong được định nghĩa bên trong `adder(..)`, vì vậy có vẻ như điều đó sẽ ngụ ý một closure duy nhất.

Nhưng thực ra, mỗi khi hàm `adder(..)` bên ngoài chạy, một thể hiện hàm `addTo(..)` bên trong *mới* được tạo ra, và cho mỗi thể hiện mới, một closure mới. Vì vậy, mỗi thể hiện hàm bên trong (được dán nhãn `add10To(..)` và `add42To(..)` trong chương trình của chúng ta) có closure riêng của nó trên thể hiện riêng của nó về môi trường phạm vi từ lần thực thi đó của `adder(..)`.

Mặc dù closure dựa trên phạm vi từ vựng, được xử lý tại thời gian biên dịch, closure được quan sát như một đặc điểm thời gian chạy của các thể hiện hàm.

### Liên Kết Trực Tiếp, Không Phải Ảnh Chụp Nhanh

Trong cả hai ví dụ từ các phần trước, chúng ta **đọc giá trị từ một biến** được giữ trong một closure. Điều đó làm cho nó cảm thấy như closure có thể là một ảnh chụp nhanh của một giá trị tại một thời điểm nhất định. Thật vậy, đó là một quan niệm sai lầm phổ biến.

Closure thực sự là một liên kết trực tiếp, bảo tồn quyền truy cập vào chính biến đầy đủ. Chúng ta không bị giới hạn chỉ đọc một giá trị; biến được đóng trên có thể được cập nhật (gán lại) nữa! Bằng cách đóng trên một biến trong một hàm, chúng ta có thể tiếp tục sử dụng biến đó (đọc và viết) miễn là tham chiếu hàm đó tồn tại trong chương trình, và từ bất cứ đâu chúng ta muốn gọi hàm đó. Đây là lý do tại sao closure là một kỹ thuật mạnh mẽ được sử dụng rộng rãi trên rất nhiều lĩnh vực lập trình!

Hình 4 mô tả các thể hiện hàm và liên kết phạm vi:

<figure>
    <img src="images/fig4.png" width="400" alt="Các thể hiện hàm được liên kết với các phạm vi thông qua closure" align="center">
    <figcaption><em>Hình 4: Hình Dung Closures</em></figcaption>
    <br><br>
</figure>

Như được hiển thị trong Hình 4, mỗi cuộc gọi đến `adder(..)` tạo ra một phạm vi BLUE(2) mới chứa một biến `num1`, cũng như một thể hiện mới của hàm `addTo(..)` như một phạm vi GREEN(3). Chú ý rằng các thể hiện hàm (`addTo10(..)` và `addTo42(..)`) có mặt trong và được gọi từ phạm vi RED(1).

Bây giờ hãy xem xét một ví dụ nơi biến được đóng trên được cập nhật:

```js
function makeCounter() {
    var count = 0;

    return function getCurrent() {
        count = count + 1;
        return count;
    };
}

var hits = makeCounter();

// sau đó

hits();     // 1

// sau đó

hits();     // 2
hits();     // 3
```

Biến `count` được đóng trên bởi hàm `getCurrent()` bên trong, giữ nó xung quanh thay vì nó phải chịu GC. Các cuộc gọi hàm `hits()` truy cập *và* cập nhật biến này, trả về một số đếm tăng dần mỗi lần.

Mặc dù phạm vi bao quanh của một closure thường là từ một hàm, điều đó thực sự không bắt buộc; chỉ cần có một hàm bên trong hiện diện bên trong một phạm vi bên ngoài:

```js
var hits;
{   // một phạm vi bên ngoài (nhưng không phải một hàm)
    let count = 0;
    hits = function getCurrent(){
        count = count + 1;
        return count;
    };
}
hits();     // 1
hits();     // 2
hits();     // 3
```

| LƯU Ý: |
| :--- |
| Tôi cố tình định nghĩa `getCurrent()` là một biểu thức `function` thay vì một khai báo `function`. Điều này không phải về closure, mà với những điều kỳ quặc nguy hiểm của FiB (Chương 6). |

Bởi vì rất phổ biến để nhầm lẫn closure là hướng giá trị thay vì hướng biến, các nhà phát triển đôi khi bị vấp ngã khi cố gắng sử dụng closure để bảo tồn ảnh chụp nhanh một giá trị từ một thời điểm nào đó. Hãy xem xét:

```js
var studentName = "Frank";

var greeting = function hello() {
    // chúng ta đang đóng trên `studentName`,
    // không phải "Frank"
    console.log(
        `Hello, ${ studentName }!`
    );
}

// sau đó

studentName = "Suzy";

// sau đó

greeting();
// Hello, Suzy!
```

Bằng cách định nghĩa `greeting()` (hay còn gọi là, `hello()`) khi `studentName` giữ giá trị `"Frank"` (trước khi gán lại thành `"Suzy"`), giả định sai lầm thường là closure sẽ bắt giữ `"Frank"`. Nhưng `greeting()` được đóng trên biến `studentName`, không phải giá trị của nó. Bất cứ khi nào `greeting()` được gọi, giá trị hiện tại của biến (`"Suzy"`, trong trường hợp này) được phản ánh.

Minh họa cổ điển về sai lầm này là định nghĩa các hàm bên trong một vòng lặp:

```js
var keeps = [];

for (var i = 0; i < 3; i++) {
    keeps[i] = function keepI(){
        // closure trên `i`
        return i;
    };
}

keeps[0]();   // 3 -- TẠI SAO!?
keeps[1]();   // 3
keeps[2]();   // 3
```

| LƯU Ý: |
| :--- |
| Loại minh họa closure này thường sử dụng `setTimeout(..)` hoặc một số callback khác như trình xử lý sự kiện, bên trong vòng lặp. Tôi đã đơn giản hóa ví dụ bằng cách lưu trữ các tham chiếu hàm trong một mảng, để chúng ta không cần xem xét thời gian bất đồng bộ trong phân tích của mình. Nguyên tắc closure là giống nhau, bất kể. |

Bạn có thể đã mong đợi cuộc gọi `keeps[0]()` trả về `0`, vì hàm đó được tạo ra trong lần lặp đầu tiên của vòng lặp khi `i` là `0`. Nhưng một lần nữa, giả định đó bắt nguồn từ việc nghĩ về closure là hướng giá trị thay vì hướng biến.

Một cái gì đó về cấu trúc của một vòng lặp `for` có thể đánh lừa chúng ta nghĩ rằng mỗi lần lặp nhận được biến `i` mới riêng của nó; trên thực tế, chương trình này chỉ có một `i` vì nó được khai báo với `var`.

Mỗi hàm được lưu trả về `3`, bởi vì vào cuối vòng lặp, biến `i` duy nhất trong chương trình đã được gán `3`. Mỗi trong ba hàm trong mảng `keeps` đều có các closure riêng lẻ, nhưng tất cả chúng đều được đóng trên cùng một biến `i` được chia sẻ đó.

Tất nhiên, một biến duy nhất chỉ có thể giữ một giá trị tại bất kỳ thời điểm nào. Vì vậy, nếu bạn muốn bảo tồn nhiều giá trị, bạn cần một biến khác nhau cho mỗi giá trị.

Làm thế nào chúng ta có thể làm điều đó trong đoạn trích vòng lặp? Hãy tạo một biến mới cho mỗi lần lặp:

```js
var keeps = [];

for (var i = 0; i < 3; i++) {
    // `j` mới được tạo mỗi lần lặp, nhận được
    // một bản sao của giá trị của `i` tại thời điểm này
    let j = i;

    // `i` ở đây không bị đóng trên, vì vậy
    // hoàn toàn ổn khi sử dụng ngay giá trị hiện tại
    // của nó trong mỗi lần lặp vòng lặp
    keeps[i] = function keepEachJ(){
        // đóng trên `j`, không phải `i`!
        return j;
    };
}
keeps[0]();   // 0
keeps[1]();   // 1
keeps[2]();   // 2
```

Mỗi hàm bây giờ được đóng trên một biến (mới) riêng biệt từ mỗi lần lặp, mặc dù tất cả chúng đều được đặt tên là `j`. Và mỗi `j` nhận được một bản sao của giá trị của `i` tại điểm đó trong lần lặp vòng lặp; `j` đó không bao giờ được gán lại. Vì vậy, cả ba hàm bây giờ trả về các giá trị mong đợi của chúng: `0`, `1`, và `2`!

Một lần nữa hãy nhớ, ngay cả khi chúng ta đang sử dụng bất đồng bộ trong chương trình này, chẳng hạn như truyền mỗi hàm `keepEachJ()` bên trong vào `setTimeout(..)` hoặc một số đăng ký trình xử lý sự kiện, cùng một loại hành vi closure vẫn sẽ được quan sát.

Nhớ lại phần "Vòng Lặp" trong Chương 5, minh họa cách một khai báo `let` trong một vòng lặp `for` thực sự tạo ra không chỉ một biến cho vòng lặp, mà thực sự tạo ra một biến mới cho *mỗi lần lặp* của vòng lặp. Thủ thuật/kỳ quặc đó chính xác là những gì chúng ta cần cho các closure vòng lặp của mình:

```js
var keeps = [];

for (let i = 0; i < 3; i++) {
    // `let i` cung cấp cho chúng ta một `i` mới cho
    // mỗi lần lặp, tự động!
    keeps[i] = function keepEachI(){
        return i;
    };
}
keeps[0]();   // 0
keeps[1]();   // 1
keeps[2]();   // 2
```

Vì chúng ta đang sử dụng `let`, ba `i` được tạo ra, một cho mỗi vòng lặp, vì vậy mỗi trong ba closure *chỉ hoạt động* như mong đợi.

### Các Closure Phổ Biến: Ajax và Sự Kiện

Closure thường gặp nhất với các callback:

```js
function lookupStudentRecord(studentID) {
    ajax(
        `https://some.api/student/${ studentID }`,
        function onRecord(record) {
            console.log(
                `${ record.name } (${ studentID })`
            );
        }
    );
}

lookupStudentRecord(114);
// Frank (114)
```

Callback `onRecord(..)` sẽ được gọi tại một thời điểm nào đó trong tương lai, sau khi phản hồi từ cuộc gọi Ajax quay trở lại. Cuộc gọi này sẽ xảy ra từ bên trong của tiện ích `ajax(..)`, bất kể nó đến từ đâu. Hơn nữa, khi điều đó xảy ra, cuộc gọi `lookupStudentRecord(..)` đã hoàn thành từ lâu.

Vậy tại sao `studentID` vẫn còn xung quanh và có thể truy cập được đối với callback? Closure.

Các trình xử lý sự kiện là một cách sử dụng phổ biến khác của closure:

```js
function listenForClicks(btn,label) {
    btn.addEventListener("click",function onClick(){
        console.log(
            `The ${ label } button was clicked!`
        );
    });
}

var submitBtn = document.getElementById("submit-btn");

listenForClicks(submitBtn,"Checkout");
```

Tham số `label` được đóng trên bởi callback trình xử lý sự kiện `onClick(..)`. Khi nút được nhấp, `label` vẫn tồn tại để được sử dụng. Đây là closure.

### Điều Gì Xảy Ra Nếu Tôi Không Thể Nhìn Thấy Nó?

Bạn có thể đã nghe câu ngạn ngữ phổ biến này:

> Nếu một cái cây đổ trong rừng nhưng không có ai ở quanh để nghe thấy nó, nó có tạo ra âm thanh không?

Đó là một chút thể dục triết học ngớ ngẩn. Tất nhiên từ quan điểm khoa học, sóng âm thanh được tạo ra. Nhưng điểm thực sự: *có quan trọng không* nếu âm thanh xảy ra?

Hãy nhớ, trọng tâm trong định nghĩa của chúng ta về closure là khả năng quan sát. Nếu một closure tồn tại (theo nghĩa kỹ thuật, triển khai, hoặc học thuật) nhưng nó không thể được quan sát trong các chương trình của chúng ta, *có quan trọng không?* Không.

Để củng cố điểm này, hãy xem một số ví dụ *không* dựa trên closure một cách có thể quan sát được.

Ví dụ, gọi một hàm sử dụng tra cứu phạm vi từ vựng:

```js
function say(myName) {
    var greeting = "Hello";
    output();

    function output() {
        console.log(
            `${ greeting }, ${ myName }!`
        );
    }
}

say("Kyle");
// Hello, Kyle!
```

Hàm bên trong `output()` truy cập các biến `greeting` và `myName` từ phạm vi bao quanh của nó. Nhưng việc gọi `output()` xảy ra trong cùng phạm vi đó, nơi tất nhiên `greeting` và `myName` vẫn có sẵn; đó chỉ là phạm vi từ vựng, không phải closure.

Bất kỳ ngôn ngữ phạm vi từ vựng nào mà các hàm của nó không hỗ trợ closure vẫn sẽ hoạt động theo cùng cách này.

Trên thực tế, các biến phạm vi toàn cục về cơ bản không thể bị đóng trên (một cách có thể quan sát được), bởi vì chúng luôn có thể truy cập được từ mọi nơi. Không có hàm nào có thể được gọi trong bất kỳ phần nào của chuỗi phạm vi mà không phải là hậu duệ của phạm vi toàn cục.

Hãy xem xét:

```js
var students = [
    { id: 14, name: "Kyle" },
    { id: 73, name: "Suzy" },
    { id: 112, name: "Frank" },
    { id: 6, name: "Sarah" }
];

function getFirstStudent() {
    return function firstStudent(){
        return students[0].name;
    };
}

var student = getFirstStudent();

student();
// Kyle
```

Hàm bên trong `firstStudent()` thực sự tham chiếu `students`, là một biến bên ngoài phạm vi riêng của nó. Nhưng vì `students` tình cờ đến từ phạm vi toàn cục, bất kể hàm đó được gọi ở đâu trong chương trình, khả năng truy cập `students` của nó không có gì đặc biệt hơn phạm vi từ vựng bình thường.

Tất cả các cuộc gọi hàm đều có thể truy cập các biến toàn cục, bất kể closure có được ngôn ngữ hỗ trợ hay không. Các biến toàn cục không cần phải được đóng trên.

Các biến chỉ đơn thuần hiện diện nhưng không bao giờ được truy cập không dẫn đến closure:

```js
function lookupStudent(studentID) {
    return function nobody(){
        var msg = "Nobody's here yet.";
        console.log(msg);
    };
}

var student = lookupStudent(112);

student();
// Nobody's here yet.
```

Hàm bên trong `nobody()` không đóng trên bất kỳ biến bên ngoài nào—nó chỉ sử dụng biến riêng của nó `msg`. Mặc dù `studentID` hiện diện trong phạm vi bao quanh, `studentID` không được tham chiếu bởi `nobody()`. Công cụ JS không cần giữ `studentID` xung quanh sau khi `lookupStudent(..)` đã chạy xong, vì vậy GC muốn dọn dẹp bộ nhớ đó!

Cho dù các hàm JS có hỗ trợ closure hay không, chương trình này sẽ hoạt động giống nhau. Do đó, không có closure được quan sát ở đây.

Nếu không có cuộc gọi hàm, closure không thể được quan sát:

```js
function greetStudent(studentName) {
    return function greeting(){
        console.log(
            `Hello, ${ studentName }!`
        );
    };
}

greetStudent("Kyle");

// không có gì khác xảy ra
```

Cái này khó, bởi vì hàm bên ngoài chắc chắn được gọi. Nhưng hàm bên trong là hàm *có thể* đã có closure, nhưng nó không bao giờ được gọi; hàm được trả về ở đây chỉ bị vứt bỏ. Vì vậy, ngay cả khi về mặt kỹ thuật công cụ JS đã tạo closure trong một khoảnh khắc ngắn, nó không được quan sát theo bất kỳ cách có ý nghĩa nào trong chương trình này.

Một cái cây có thể đã đổ... nhưng chúng ta không nghe thấy nó, vì vậy chúng ta không quan tâm.

### Định Nghĩa Có Thể Quan Sát

Bây giờ chúng ta đã sẵn sàng để định nghĩa closure:

> Closure được quan sát khi một hàm sử dụng (các) biến từ (các) phạm vi bên ngoài ngay cả khi đang chạy trong một phạm vi nơi (các) biến đó sẽ không thể truy cập được.

Các phần chính của định nghĩa này là:

* Phải có một hàm tham gia

* Phải tham chiếu ít nhất một biến từ một phạm vi bên ngoài

* Phải được gọi trong một nhánh khác của chuỗi phạm vi so với (các) biến

Định nghĩa hướng quan sát này có nghĩa là chúng ta không nên bác bỏ closure như một chuyện vặt vãnh gián tiếp, học thuật. Thay vào đó, chúng ta nên xem xét và lập kế hoạch cho các hiệu ứng trực tiếp, cụ thể mà closure có đối với hành vi chương trình của chúng ta.

## Vòng Đời Closure và Thu Gom Rác (GC)

Vì closure vốn gắn liền với một thể hiện hàm, closure của nó trên một biến kéo dài chừng nào vẫn còn một tham chiếu đến hàm đó.

Nếu mười hàm đều đóng trên cùng một biến, và theo thời gian chín trong số các tham chiếu hàm này bị loại bỏ, tham chiếu hàm duy nhất còn lại vẫn bảo tồn biến đó. Một khi tham chiếu hàm cuối cùng đó bị loại bỏ, closure cuối cùng trên biến đó biến mất, và chính biến đó bị GC'd.

Điều này có tác động quan trọng đến việc xây dựng các chương trình hiệu quả và hiệu suất. Closure có thể ngăn chặn GC của một biến mà bạn đã xong việc một cách bất ngờ, dẫn đến việc sử dụng bộ nhớ chạy trốn theo thời gian. Đó là lý do tại sao quan trọng là phải loại bỏ các tham chiếu hàm (và do đó các closure của chúng) khi chúng không còn cần thiết nữa.

Hãy xem xét:

```js
function manageBtnClickEvents(btn) {
    var clickHandlers = [];

    return function listener(cb){
        if (cb) {
            let clickHandler =
                function onClick(evt){
                    console.log("clicked!");
                    cb(evt);
                };
            clickHandlers.push(clickHandler);
            btn.addEventListener(
                "click",
                clickHandler
            );
        }
        else {
            // truyền không có callback hủy đăng ký
            // tất cả các trình xử lý nhấp chuột
            for (let handler of clickHandlers) {
                btn.removeEventListener(
                    "click",
                    handler
                );
            }

            clickHandlers = [];
        }
    };
}

// var mySubmitBtn = ..
var onSubmit = manageBtnClickEvents(mySubmitBtn);

onSubmit(function checkout(evt){
    // xử lý thanh toán
});

onSubmit(function trackAction(evt){
    // ghi lại hành động vào phân tích
});

// sau đó, hủy đăng ký tất cả các trình xử lý:
onSubmit();
```

Trong chương trình này, hàm `onClick(..)` bên trong giữ một closure trên `cb` được truyền vào (callback sự kiện được cung cấp). Điều đó có nghĩa là các tham chiếu biểu thức hàm `checkout()` và `trackAction()` được giữ thông qua closure (và không thể bị GC'd) chừng nào các trình xử lý sự kiện này được đăng ký.

Khi chúng ta gọi `onSubmit()` không có đầu vào ở dòng cuối cùng, tất cả các trình xử lý sự kiện bị hủy đăng ký, và mảng `clickHandlers` bị làm trống. Một khi tất cả các tham chiếu hàm trình xử lý nhấp chuột bị loại bỏ, các closure của các tham chiếu `cb` đến `checkout()` và `trackAction()` bị loại bỏ.

Khi xem xét sức khỏe tổng thể và hiệu quả của chương trình, việc hủy đăng ký một trình xử lý sự kiện khi nó không còn cần thiết có thể còn quan trọng hơn cả việc đăng ký ban đầu!

### Theo Biến hay Theo Phạm Vi?

Một câu hỏi khác chúng ta cần giải quyết: chúng ta nên nghĩ về closure như chỉ áp dụng cho (các) biến bên ngoài được tham chiếu, hay closure bảo tồn toàn bộ chuỗi phạm vi với tất cả các biến của nó?

Nói cách khác, trong đoạn trích đăng ký sự kiện trước đó, hàm `onClick(..)` bên trong có được đóng trên chỉ `cb`, hay nó cũng được đóng trên `clickHandler`, `clickHandlers`, và `btn`?

Về mặt khái niệm, closure là **theo biến** thay vì *theo phạm vi*. Các callback Ajax, trình xử lý sự kiện, và tất cả các hình thức closure hàm khác thường được giả định chỉ đóng trên những gì chúng tham chiếu rõ ràng.

Nhưng thực tế phức tạp hơn thế.

Một chương trình khác để xem xét:

```js
function manageStudentGrades(studentRecords) {
    var grades = studentRecords.map(getGrade);

    return addGrade;

    // ************************

    function getGrade(record){
        return record.grade;
    }

    function sortAndTrimGradesList() {
        // sắp xếp theo điểm, giảm dần
        grades.sort(function desc(g1,g2){
            return g2 - g1;
        });

        // chỉ giữ lại 10 điểm cao nhất
        grades = grades.slice(0,10);
    }

    function addGrade(newGrade) {
        grades.push(newGrade);
        sortAndTrimGradesList();
        return grades;
    }
}

var addNextGrade = manageStudentGrades([
    { id: 14, name: "Kyle", grade: 86 },
    { id: 73, name: "Suzy", grade: 87 },
    { id: 112, name: "Frank", grade: 75 },
    // ..nhiều bản ghi hơn..
    { id: 6, name: "Sarah", grade: 91 }
]);

// sau đó

addNextGrade(81);
addNextGrade(68);
// [ .., .., ... ]
```

Hàm bên ngoài `manageStudentGrades(..)` nhận một danh sách các bản ghi sinh viên, và trả về một tham chiếu hàm `addGrade(..)`, mà chúng ta dán nhãn bên ngoài là `addNextGrade(..)`. Mỗi lần chúng ta gọi `addNextGrade(..)` với một điểm mới, chúng ta nhận lại một danh sách hiện tại của 10 điểm cao nhất, được sắp xếp theo số giảm dần (xem `sortAndTrimGradesList()`).

Từ cuối cuộc gọi `manageStudentGrades(..)` ban đầu, và giữa nhiều cuộc gọi `addNextGrade(..)`, biến `grades` được bảo tồn bên trong `addGrade(..)` thông qua closure; đó là cách danh sách điểm cao nhất đang chạy được duy trì. Hãy nhớ, đó là một closure trên chính biến `grades`, không phải mảng mà nó giữ.

Tuy nhiên, đó không phải là closure duy nhất liên quan. Bạn có thể phát hiện các biến khác đang bị đóng trên không?

Bạn có phát hiện ra rằng `addGrade(..)` tham chiếu `sortAndTrimGradesList` không? Điều đó có nghĩa là nó cũng được đóng trên định danh đó, tình cờ giữ một tham chiếu đến hàm `sortAndTrimGradesList()`. Hàm bên trong thứ hai đó phải ở lại để `addGrade(..)` có thể tiếp tục gọi nó, điều này cũng có nghĩa là bất kỳ biến nào *nó* đóng trên cũng ở lại—mặc dù, trong trường hợp này, không có gì thêm được đóng trên ở đó.

Cái gì khác được đóng trên?

Hãy xem xét biến `getGrade` (và hàm của nó); nó có bị đóng trên không? Nó được tham chiếu trong phạm vi bên ngoài của `manageStudentGrades(..)` trong cuộc gọi `.map(getGrade)`. Nhưng nó không được tham chiếu trong `addGrade(..)` hoặc `sortAndTrimGradesList()`.

Còn về danh sách (có khả năng) lớn các bản ghi sinh viên mà chúng ta truyền vào dưới dạng `studentRecords` thì sao? Biến đó có bị đóng trên không? Nếu có, mảng các bản ghi sinh viên không bao giờ bị GC'd, dẫn đến chương trình này giữ một lượng bộ nhớ lớn hơn chúng ta có thể giả định. Nhưng nếu chúng ta nhìn kỹ lại, không có hàm bên trong nào tham chiếu `studentRecords`.

Theo định nghĩa *theo biến* của closure, vì `getGrade` và `studentRecords` *không* được tham chiếu bởi các hàm bên trong, chúng không bị đóng trên. Chúng nên có sẵn miễn phí cho GC ngay sau khi cuộc gọi `manageStudentGrades(..)` hoàn thành.

Thật vậy, hãy thử gỡ lỗi mã này trong một công cụ JS gần đây, như v8 trong Chrome, đặt một điểm ngắt bên trong hàm `addGrade(..)`. Bạn có thể nhận thấy rằng trình kiểm tra **không** liệt kê biến `studentRecords`. Đó là bằng chứng, về mặt gỡ lỗi, rằng công cụ không duy trì `studentRecords` thông qua closure. Phù!

Nhưng quan sát này đáng tin cậy đến mức nào như bằng chứng? Hãy xem xét chương trình (khá gượng ép!) này:

```js
function storeStudentInfo(id,name,grade) {
    return function getInfo(whichValue){
        // cảnh báo:
        //   sử dụng `eval(..)` là một ý tưởng tồi!
        var val = eval(whichValue);
        return val;
    };
}

var info = storeStudentInfo(73,"Suzy",87);

info("name");
// Suzy

info("grade");
// 87
```

Chú ý rằng hàm bên trong `getInfo(..)` không được đóng trên rõ ràng bất kỳ biến `id`, `name`, hoặc `grade` nào. Tuy nhiên, các cuộc gọi đến `info(..)` dường như vẫn có thể truy cập các biến, mặc dù thông qua việc sử dụng gian lận phạm vi từ vựng `eval(..)` (xem Chương 1).

Vì vậy, tất cả các biến chắc chắn đã được bảo tồn thông qua closure, mặc dù không được tham chiếu rõ ràng bởi hàm bên trong. Vậy điều đó có bác bỏ khẳng định *theo biến* ủng hộ *theo phạm vi* không? Tùy thuộc.

Nhiều công cụ JS hiện đại áp dụng một *tối ưu hóa* loại bỏ bất kỳ biến nào khỏi phạm vi closure mà không được tham chiếu rõ ràng. Tuy nhiên, như chúng ta thấy với `eval(..)`, có những tình huống mà tối ưu hóa như vậy không thể được áp dụng, và phạm vi closure tiếp tục chứa tất cả các biến ban đầu của nó. Nói cách khác, closure phải là *theo phạm vi*, về mặt triển khai, và sau đó một tối ưu hóa tùy chọn cắt giảm phạm vi xuống chỉ những gì đã được đóng trên (một kết quả tương tự như closure *theo biến*).

Ngay cả gần đây như vài năm trước, nhiều công cụ JS đã không áp dụng tối ưu hóa này; có thể các trang web của bạn vẫn chạy trong các trình duyệt như vậy, đặc biệt là trên các thiết bị cũ hơn hoặc cấp thấp hơn. Điều đó có nghĩa là có thể các closure sống lâu như trình xử lý sự kiện có thể đang giữ bộ nhớ lâu hơn nhiều so với chúng ta giả định.

Và thực tế là nó là một tối ưu hóa tùy chọn ngay từ đầu, thay vì một yêu cầu của đặc tả, có nghĩa là chúng ta không nên chỉ tình cờ giả định quá mức khả năng áp dụng của nó.

Trong các trường hợp một biến giữ một giá trị lớn (như một đối tượng hoặc mảng) và biến đó có mặt trong một phạm vi closure, nếu bạn không cần giá trị đó nữa và không muốn bộ nhớ đó bị giữ, an toàn hơn (sử dụng bộ nhớ) để loại bỏ giá trị thủ công thay vì dựa vào tối ưu hóa closure/GC.

Hãy áp dụng một *bản sửa lỗi* cho ví dụ `manageStudentGrades(..)` trước đó để đảm bảo mảng có khả năng lớn được giữ trong `studentRecords` không bị kẹt trong một phạm vi closure một cách không cần thiết:

```js
function manageStudentGrades(studentRecords) {
    var grades = studentRecords.map(getGrade);

    // bỏ đặt `studentRecords` để ngăn chặn
    // giữ lại bộ nhớ không mong muốn trong closure
    studentRecords = null;

    return addGrade;
    // ..
}
```

Chúng ta không loại bỏ `studentRecords` khỏi phạm vi closure; điều đó chúng ta không thể kiểm soát. Chúng ta đang đảm bảo rằng ngay cả khi `studentRecords` vẫn còn trong phạm vi closure, biến đó không còn tham chiếu đến mảng dữ liệu có khả năng lớn nữa; mảng có thể bị GC'd.

Một lần nữa, trong nhiều trường hợp JS có thể tự động tối ưu hóa chương trình để có cùng hiệu quả. Nhưng vẫn là một thói quen tốt để cẩn thận và đảm bảo rõ ràng chúng ta không giữ bất kỳ lượng bộ nhớ thiết bị đáng kể nào bị ràng buộc lâu hơn mức cần thiết.

Thực tế là, chúng ta cũng về mặt kỹ thuật không cần hàm `getGrade()` nữa sau khi cuộc gọi `.map(getGrade)` hoàn thành. Nếu hồ sơ ứng dụng của chúng ta cho thấy đây là một khu vực quan trọng của việc sử dụng bộ nhớ dư thừa, chúng ta có thể có thể tiết kiệm thêm một chút bộ nhớ bằng cách giải phóng tham chiếu đó để giá trị của nó cũng không bị ràng buộc. Điều đó có thể không cần thiết trong ví dụ đồ chơi này, nhưng đây là một kỹ thuật chung cần ghi nhớ nếu bạn đang tối ưu hóa dấu chân bộ nhớ của ứng dụng của mình.

Điều rút ra: quan trọng là phải biết nơi các closure xuất hiện trong các chương trình của chúng ta, và những biến nào được bao gồm. Chúng ta nên quản lý các closure này cẩn thận để chúng ta chỉ giữ lại những gì cần thiết tối thiểu và không lãng phí bộ nhớ.

## Một Quan Điểm Thay Thế

Xem xét lại định nghĩa làm việc của chúng ta cho closure, khẳng định là các hàm là "giá trị hạng nhất" có thể được truyền xung quanh chương trình, giống như bất kỳ giá trị nào khác. Closure là liên kết-kết hợp kết nối hàm đó với phạm vi/biến bên ngoài chính nó, bất kể hàm đó đi đâu.

Hãy nhớ lại một ví dụ mã từ đầu chương này, một lần nữa với các màu bong bóng phạm vi liên quan được chú thích:

```js
// phạm vi bên ngoài/toàn cục: RED(1)

function adder(num1) {
    // phạm vi hàm: BLUE(2)

    return function addTo(num2){
        // phạm vi hàm: GREEN(3)

        return num1 + num2;
    };
}

var add10To = adder(10);
var add42To = adder(42);

add10To(15);    // 25
add42To(9);     // 51
```

Quan điểm hiện tại của chúng ta gợi ý rằng bất cứ nơi nào một hàm được truyền và gọi, closure bảo tồn một liên kết ẩn trở lại phạm vi ban đầu để tạo điều kiện cho việc truy cập vào các biến được đóng trên. Hình 4, lặp lại ở đây để thuận tiện, minh họa khái niệm này:

<figure>
    <img src="images/fig4.png" width="400" alt="Các thể hiện hàm được liên kết với các phạm vi thông qua closure" align="center">
    <figcaption><em>Hình 4 (lặp lại): Hình Dung Closures</em></figcaption>
    <br><br>
</figure>

Nhưng có một cách khác để suy nghĩ về closure, và chính xác hơn là bản chất của các hàm được *truyền xung quanh*, có thể giúp làm sâu sắc thêm các mô hình tinh thần.

Mô hình thay thế này giảm nhấn mạnh "các hàm như giá trị hạng nhất," và thay vào đó nắm lấy cách các hàm (như tất cả các giá trị không nguyên thủy) được giữ bằng tham chiếu trong JS, và được gán/truyền bằng sao chép tham chiếu—xem Phụ lục A của cuốn sách *Get Started* để biết thêm thông tin.

Thay vì nghĩ về thể hiện hàm bên trong của `addTo(..)` di chuyển đến phạm vi RED(1) bên ngoài thông qua `return` và gán, chúng ta có thể hình dung rằng các thể hiện hàm thực sự chỉ ở lại vị trí trong môi trường phạm vi riêng của chúng, tất nhiên với chuỗi phạm vi của chúng nguyên vẹn.

Những gì được *gửi* đến phạm vi RED(1) là **chỉ một tham chiếu** đến thể hiện hàm tại chỗ, thay vì chính thể hiện hàm. Hình 5 mô tả các thể hiện hàm bên trong vẫn ở vị trí, được trỏ đến bởi các tham chiếu RED(1) `addTo10` và `addTo42`, tương ứng:

<figure>
    <img src="images/fig5.png" width="400" alt="Các thể hiện hàm bên trong các phạm vi thông qua closure, được liên kết bởi các tham chiếu" align="center">
    <figcaption><em>Hình 5: Hình Dung Closures (Thay Thế)</em></figcaption>
    <br><br>
</figure>

Như được hiển thị trong Hình 5, mỗi cuộc gọi đến `adder(..)` vẫn tạo ra một phạm vi BLUE(2) mới chứa một biến `num1`, cũng như một thể hiện của phạm vi GREEN(3) `addTo(..)`. Nhưng điều khác biệt so với Hình 4 là, bây giờ các thể hiện GREEN(3) này vẫn ở vị trí, lồng nhau một cách tự nhiên bên trong các thể hiện phạm vi BLUE(2) của chúng. Các tham chiếu `addTo10` và `addTo42` được di chuyển đến phạm vi bên ngoài RED(1), không phải chính các thể hiện hàm.

Khi `addTo10(15)` được gọi, thể hiện hàm `addTo(..)` (vẫn ở vị trí trong môi trường phạm vi BLUE(2) ban đầu của nó) được gọi. Vì chính thể hiện hàm không bao giờ di chuyển, tất nhiên nó vẫn có quyền truy cập tự nhiên vào chuỗi phạm vi của nó. Tương tự với cuộc gọi `addTo42(9)`—không có gì đặc biệt ở đây ngoài phạm vi từ vựng.

Vậy thì *là* closure, nếu không phải là *phép thuật* cho phép một hàm duy trì một liên kết đến chuỗi phạm vi ban đầu của nó ngay cả khi hàm đó di chuyển xung quanh trong các phạm vi khác? Trong mô hình thay thế này, các hàm ở lại vị trí và tiếp tục truy cập chuỗi phạm vi ban đầu của chúng giống như chúng luôn có thể.

Closure thay vào đó mô tả *phép thuật* của việc **giữ cho một thể hiện hàm sống**, cùng với toàn bộ môi trường phạm vi và chuỗi của nó, chừng nào còn ít nhất một tham chiếu đến thể hiện hàm đó trôi nổi trong bất kỳ phần nào khác của chương trình.

Định nghĩa đó về closure ít quan sát hơn và nghe có vẻ ít quen thuộc hơn một chút so với quan điểm học thuật truyền thống. Nhưng nó vẫn hữu ích, bởi vì lợi ích là chúng ta đơn giản hóa việc giải thích closure thành một sự kết hợp đơn giản của các tham chiếu và các thể hiện hàm tại chỗ.

Mô hình trước đó (Hình 4) không *sai* khi mô tả closure trong JS. Nó chỉ được truyền cảm hứng về mặt khái niệm hơn, một quan điểm học thuật về closure. Ngược lại, mô hình thay thế (Hình 5) có thể được mô tả là tập trung vào triển khai hơn một chút, cách JS thực sự hoạt động.

Cả hai quan điểm/mô hình đều hữu ích trong việc hiểu closure, nhưng người đọc có thể thấy cái này dễ nắm bắt hơn cái kia một chút. Dù bạn chọn cái nào, kết quả có thể quan sát được trong chương trình của chúng ta là giống nhau.

| LƯU Ý: |
| :--- |
| Mô hình thay thế này cho closure có ảnh hưởng đến việc liệu chúng ta phân loại các callback đồng bộ là ví dụ về closure hay không. Thêm về sắc thái này trong Phụ lục A. |

## Tại Sao Closure?

Bây giờ chúng ta đã có một cảm giác toàn diện về closure là gì và nó hoạt động như thế nào, hãy khám phá một số cách nó có thể cải thiện cấu trúc mã và tổ chức của một chương trình ví dụ.

Hãy tưởng tượng bạn có một nút trên một trang mà khi được nhấp, nên truy xuất và gửi một số dữ liệu thông qua một yêu cầu Ajax. Không sử dụng closure:

```js
var APIendpoints = {
    studentIDs:
        "https://some.api/register-students",
    // ..
};

var data = {
    studentIDs: [ 14, 73, 112, 6 ],
    // ..
};

function makeRequest(evt) {
    var btn = evt.target;
    var recordKind = btn.dataset.kind;
    ajax(
        APIendpoints[recordKind],
        data[recordKind]
    );
}

// <button data-kind="studentIDs">
//    Register Students
// </button>
btn.addEventListener("click",makeRequest);
```

Tiện ích `makeRequest(..)` chỉ nhận một đối tượng `evt` từ một sự kiện nhấp chuột. Từ đó, nó phải truy xuất thuộc tính `data-kind` từ phần tử nút mục tiêu, và sử dụng giá trị đó để tra cứu cả URL cho điểm cuối API cũng như dữ liệu nào nên được bao gồm trong yêu cầu Ajax.

Điều này hoạt động OK, nhưng thật không may (kém hiệu quả, khó hiểu hơn) khi trình xử lý sự kiện phải đọc một thuộc tính DOM mỗi khi nó được kích hoạt. Tại sao một trình xử lý sự kiện không thể *nhớ* giá trị này? Hãy thử sử dụng closure để cải thiện mã:

```js
var APIendpoints = {
    studentIDs:
        "https://some.api/register-students",
    // ..
};

var data = {
    studentIDs: [ 14, 73, 112, 6 ],
    // ..
};

function setupButtonHandler(btn) {
    var recordKind = btn.dataset.kind;

    btn.addEventListener(
        "click",
        function makeRequest(evt){
            ajax(
                APIendpoints[recordKind],
                data[recordKind]
            );
        }
    );
}

// <button data-kind="studentIDs">
//    Register Students
// </button>

setupButtonHandler(btn);
```

Với cách tiếp cận `setupButtonHandler(..)`, thuộc tính `data-kind` được truy xuất một lần và gán cho biến `recordKind` tại thiết lập ban đầu. `recordKind` sau đó được đóng trên bởi trình xử lý nhấp chuột `makeRequest(..)` bên trong, và giá trị của nó được sử dụng trên mỗi lần kích hoạt sự kiện để tra cứu URL và dữ liệu nên được gửi.

| LƯU Ý: |
| :--- |
| `evt` vẫn được truyền cho `makeRequest(..)`, mặc dù trong trường hợp này chúng ta không sử dụng nó nữa. Nó vẫn được liệt kê, để nhất quán với đoạn trích trước. |

Bằng cách đặt `recordKind` bên trong `setupButtonHandler(..)`, chúng ta hạn chế sự phơi bày phạm vi của biến đó cho một tập hợp con thích hợp hơn của chương trình; lưu trữ nó toàn cục sẽ tồi tệ hơn cho tổ chức mã và khả năng đọc. Closure cho phép thể hiện hàm `makeRequest()` bên trong *nhớ* biến này và truy cập bất cứ khi nào nó cần.

Xây dựng trên mẫu này, chúng ta có thể đã tra cứu cả URL và dữ liệu một lần, tại thiết lập:

```js
function setupButtonHandler(btn) {
    var recordKind = btn.dataset.kind;
    var requestURL = APIendpoints[recordKind];
    var requestData = data[recordKind];

    btn.addEventListener(
        "click",
        function makeRequest(evt){
            ajax(requestURL,requestData);
        }
    );
}
```

Bây giờ `makeRequest(..)` được đóng trên `requestURL` và `requestData`, điều này sạch hơn một chút để hiểu, và cũng hiệu quả hơn một chút.

Hai kỹ thuật tương tự từ mô hình Lập trình Hàm (FP) dựa vào closure là ứng dụng một phần (partial application) và currying. Tóm lại, với các kỹ thuật này, chúng ta thay đổi *hình dạng* của các hàm yêu cầu nhiều đầu vào để một số đầu vào được cung cấp trước, và các đầu vào khác được cung cấp sau; các đầu vào ban đầu được nhớ thông qua closure. Một khi tất cả các đầu vào đã được cung cấp, hành động cơ bản được thực hiện.

Bằng cách tạo một thể hiện hàm đóng gói một số thông tin bên trong (thông qua closure), hàm-với-thông-tin-được-lưu-trữ sau đó có thể được sử dụng trực tiếp mà không cần cung cấp lại đầu vào đó. Điều này làm cho phần mã đó sạch hơn, và cũng cung cấp cơ hội để dán nhãn các hàm được ứng dụng một phần với các tên ngữ nghĩa tốt hơn.

Thích ứng ứng dụng một phần, chúng ta có thể cải thiện thêm mã trước đó:

```js
function defineHandler(requestURL,requestData) {
    return function makeRequest(evt){
        ajax(requestURL,requestData);
    };
}

function setupButtonHandler(btn) {
    var recordKind = btn.dataset.kind;
    var handler = defineHandler(
        APIendpoints[recordKind],
        data[recordKind]
    );
    btn.addEventListener("click",handler);
}
```

Các đầu vào `requestURL` và `requestData` được cung cấp trước thời hạn, dẫn đến hàm được ứng dụng một phần `makeRequest(..)`, mà chúng ta dán nhãn cục bộ là `handler`. Khi sự kiện cuối cùng kích hoạt, đầu vào cuối cùng (`evt`, ngay cả khi nó bị bỏ qua) được truyền cho `handler()`, hoàn thành các đầu vào của nó và kích hoạt yêu cầu Ajax cơ bản.

Về mặt hành vi, chương trình này khá giống với chương trình trước, với cùng loại closure. Nhưng bằng cách cô lập việc tạo `makeRequest(..)` trong một tiện ích riêng biệt (`defineHandler(..)`), chúng ta làm cho định nghĩa đó có thể tái sử dụng nhiều hơn trên toàn bộ chương trình. Chúng ta cũng giới hạn rõ ràng phạm vi closure chỉ cho hai biến cần thiết.

## Gần Hơn Với Closure

Khi chúng ta đóng lại một chương dày đặc, hãy hít thở sâu để tất cả chìm vào. Nghiêm túc mà nói, đó là rất nhiều thông tin cho bất cứ ai tiêu thụ!

Chúng ta đã khám phá hai mô hình để giải quyết closure về mặt tinh thần:

* Quan sát: closure là một thể hiện hàm nhớ các biến bên ngoài của nó ngay cả khi hàm đó được truyền đến và **được gọi trong** các phạm vi khác.

* Triển khai: closure là một thể hiện hàm và môi trường phạm vi của nó được bảo tồn tại chỗ trong khi bất kỳ tham chiếu nào đến nó được truyền xung quanh và **được gọi từ** các phạm vi khác.

Tóm tắt các lợi ích cho các chương trình của chúng ta:

* Closure có thể cải thiện hiệu quả bằng cách cho phép một thể hiện hàm nhớ thông tin đã xác định trước đó thay vì phải tính toán nó mỗi lần.

* Closure có thể cải thiện khả năng đọc mã, giới hạn phơi bày phạm vi bằng cách đóng gói (các) biến bên trong các thể hiện hàm, trong khi vẫn đảm bảo thông tin trong các biến đó có thể truy cập được cho việc sử dụng trong tương lai. Các thể hiện hàm hẹp hơn, chuyên biệt hơn kết quả sạch hơn để tương tác, vì thông tin được bảo tồn không cần phải được truyền vào mỗi lần gọi.

Trước khi bạn tiếp tục, hãy dành chút thời gian để trình bày lại tóm tắt này *bằng lời của riêng bạn*, giải thích closure là gì và tại sao nó hữu ích trong các chương trình của bạn. Văn bản sách chính kết thúc với một chương cuối cùng xây dựng trên closure với mẫu module.
