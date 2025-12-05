# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Chương 3: Chuỗi Phạm Vi

Chương 1 và 2 đã đặt ra một định nghĩa cụ thể về *phạm vi từ vựng* (và các phần của nó) và minh họa các phép ẩn dụ hữu ích cho nền tảng khái niệm của nó. Trước khi tiếp tục với chương này, hãy tìm người khác để giải thích (bằng văn bản hoặc nói), bằng lời của riêng bạn, phạm vi từ vựng là gì và tại sao việc hiểu nó lại hữu ích.

Điều đó có vẻ như là một bước bạn có thể bỏ qua, nhưng tôi thấy nó thực sự giúp dành thời gian để tái cấu trúc những ý tưởng này thành lời giải thích cho người khác. Điều đó giúp bộ não của chúng ta tiêu hóa những gì chúng ta đang học!

Bây giờ là lúc đào sâu vào các chi tiết cụ thể, vì vậy hãy mong đợi rằng mọi thứ sẽ chi tiết hơn nhiều từ đây trở đi. Tuy nhiên, hãy kiên trì, bởi vì những cuộc thảo luận này thực sự nhấn mạnh việc chúng ta tất cả *không biết* bao nhiêu về phạm vi. Hãy chắc chắn dành thời gian của bạn với văn bản và tất cả các đoạn mã được cung cấp.

Để làm mới ngữ cảnh của ví dụ đang chạy của chúng ta, hãy nhớ lại hình minh họa được mã hóa màu của các bong bóng phạm vi lồng nhau, từ Chương 2, Hình 2:

<figure>
    <img src="images/fig2.png" width="500" alt="Colored Scope Bubbles" align="center">
    <figcaption><em>Hình 2 (Ch. 2): Bong Bóng Phạm Vi Màu</em></figcaption>
    <br><br>
</figure>

Các kết nối giữa các phạm vi được lồng trong các phạm vi khác được gọi là chuỗi phạm vi, xác định đường dẫn mà các biến có thể được truy cập. Chuỗi được định hướng, có nghĩa là tra cứu chỉ di chuyển lên/ra ngoài.

## "Tra Cứu" Chủ Yếu Là Khái Niệm

Trong Hình 2, hãy chú ý màu của tham chiếu biến `students` trong vòng lặp `for`. Chính xác chúng ta đã xác định nó là viên bi ĐỎ(1) như thế nào?

Trong Chương 2, chúng ta đã mô tả truy cập thời gian chạy của một biến như một "tra cứu", trong đó *Engine* phải bắt đầu bằng cách hỏi *Scope Manager* của phạm vi hiện tại nếu nó biết về một định danh/biến, và tiến lên/ra ngoài qua chuỗi các phạm vi lồng nhau (hướng tới phạm vi toàn cục) cho đến khi tìm thấy, nếu có. Tra cứu dừng ngay khi tìm thấy khai báo tên khớp đầu tiên trong một xô phạm vi.

Quá trình tra cứu do đó xác định rằng `students` là viên bi ĐỎ(1), bởi vì chúng ta chưa tìm thấy tên biến khớp khi chúng ta đi qua chuỗi phạm vi, cho đến khi chúng ta đến phạm vi toàn cục ĐỎ(1) cuối cùng.

Tương tự, `studentID` trong câu lệnh `if` được xác định là viên bi XANH DƯƠNG(2).

Gợi ý về quá trình tra cứu thời gian chạy này hoạt động tốt cho sự hiểu biết khái niệm, nhưng nó không thực sự là cách mọi thứ thường hoạt động trong thực tế.

Màu của xô của viên bi (hay còn gọi là thông tin meta về phạm vi nào mà một biến bắt nguồn từ đó) *thường được xác định* trong quá trình xử lý biên dịch ban đầu. Bởi vì phạm vi từ vựng khá nhiều được hoàn thiện tại thời điểm đó, màu của viên bi sẽ không thay đổi dựa trên bất cứ điều gì có thể xảy ra sau này trong thời gian chạy.

Vì màu của viên bi được biết từ biên dịch, và nó bất biến, thông tin này có thể sẽ được lưu trữ với (hoặc ít nhất là có thể truy cập từ) mục nhập của mỗi biến trong AST; thông tin đó sau đó được sử dụng rõ ràng bởi các hướng dẫn thực thi tạo thành thời gian chạy của chương trình.

Nói cách khác, *Engine* (từ Chương 2) không cần tra cứu qua một loạt các phạm vi để tìm ra xô phạm vi nào mà một biến đến từ đó. Thông tin đó đã được biết! Tránh nhu cầu tra cứu thời gian chạy là một lợi ích tối ưu hóa chính của phạm vi từ vựng. Thời gian chạy hoạt động hiệu quả hơn mà không dành thời gian cho tất cả các tra cứu này.

Nhưng tôi đã nói "...thường được xác định..." chỉ một lúc trước, liên quan đến việc tìm ra màu của viên bi trong quá trình biên dịch. Vậy trong trường hợp nào nó sẽ *không* được biết trong quá trình biên dịch?

Hãy xem xét một tham chiếu đến một biến không được khai báo trong bất kỳ phạm vi có sẵn từ vựng nào trong tệp hiện tại—xem *Get Started*, Chương 1, khẳng định rằng mỗi tệp là chương trình riêng biệt của nó từ quan điểm biên dịch JS. Nếu không tìm thấy khai báo, điều đó không *nhất thiết* là một lỗi. Một tệp khác (chương trình) trong thời gian chạy thực sự có thể khai báo biến đó trong phạm vi toàn cục được chia sẻ.

Vì vậy, xác định cuối cùng về việc liệu biến đã từng được khai báo đúng cách trong một số xô có thể truy cập có thể cần được hoãn lại cho thời gian chạy.

Bất kỳ tham chiếu nào đến một biến ban đầu *chưa được khai báo* được để lại như một viên bi không màu trong quá trình biên dịch tệp đó; màu này không thể được xác định cho đến khi các tệp liên quan khác đã được biên dịch và thời gian chạy ứng dụng bắt đầu. Tra cứu hoãn lại đó cuối cùng sẽ giải quyết màu thành phạm vi nào mà biến được tìm thấy (có thể là phạm vi toàn cục).

Tuy nhiên, tra cứu này sẽ chỉ cần thiết nhiều nhất một lần cho mỗi biến, vì không có gì khác trong thời gian chạy có thể thay đổi màu của viên bi đó sau này.

Phần "Thất Bại Tra Cứu" trong Chương 2 đề cập đến những gì xảy ra nếu một viên bi cuối cùng vẫn không có màu tại thời điểm tham chiếu của nó được thực thi thời gian chạy.

## Che Khuất (Shadowing)

"Shadowing" có thể nghe có vẻ bí ẩn và hơi đáng ngờ. Nhưng đừng lo lắng, nó hoàn toàn hợp pháp!

Ví dụ đang chạy của chúng ta cho các chương này sử dụng các tên biến khác nhau qua các ranh giới phạm vi. Vì tất cả chúng đều có tên duy nhất, theo một cách nào đó sẽ không quan trọng nếu tất cả chúng chỉ được lưu trữ trong một xô (như ĐỎ(1)).

Nơi có các xô phạm vi từ vựng khác nhau bắt đầu quan trọng hơn là khi bạn có hai hoặc nhiều biến, mỗi biến trong các phạm vi khác nhau, với cùng tên từ vựng. Một phạm vi duy nhất không thể có hai hoặc nhiều biến có cùng tên; các tham chiếu nhiều như vậy sẽ được giả định là chỉ một biến.

Vì vậy, nếu bạn cần duy trì hai hoặc nhiều biến có cùng tên, bạn phải sử dụng các phạm vi riêng biệt (thường là lồng nhau). Và trong trường hợp đó, cách các xô phạm vi khác nhau được bố trí rất liên quan.

Hãy xem xét:

```js
var studentName = "Suzy";

function printStudent(studentName) {
    studentName = studentName.toUpperCase();
    console.log(studentName);
}

printStudent("Frank");
// FRANK

printStudent(studentName);
// SUZY

console.log(studentName);
// Suzy
```

| MẸO: |
| :--- |
| Trước khi bạn tiếp tục, hãy dành thời gian để phân tích mã này bằng các kỹ thuật/phép ẩn dụ khác nhau mà chúng ta đã đề cập trong cuốn sách. Đặc biệt, hãy chắc chắn xác định màu bi/bong bóng trong đoạn mã này. Đó là thực hành tốt! |

Biến `studentName` trên dòng 1 (câu lệnh `var studentName = ..`) tạo ra một viên bi ĐỎ(1). Cùng biến có tên được khai báo là viên bi XANH DƯƠNG(2) trên dòng 3, tham số trong định nghĩa hàm `printStudent(..)`.

Viên bi `studentName` sẽ có màu gì trong câu lệnh gán `studentName = studentName.toUpperCase()` và câu lệnh `console.log(studentName)`? Cả ba tham chiếu `studentName` sẽ là XANH DƯƠNG(2).

Với khái niệm "tra cứu", chúng ta khẳng định rằng nó bắt đầu với phạm vi hiện tại và hoạt động theo cách của nó ra ngoài/lên, dừng ngay khi tìm thấy biến khớp. `studentName` XANH DƯƠNG(2) được tìm thấy ngay lập tức. `studentName` ĐỎ(1) thậm chí không bao giờ được xem xét.

Đây là một khía cạnh chính của hành vi phạm vi từ vựng, được gọi là *shadowing* (che khuất). Biến `studentName` XANH DƯƠNG(2) (tham số) che khuất `studentName` ĐỎ(1). Vì vậy, tham số đang che khuất biến toàn cục (bị che khuất). Lặp lại câu đó cho chính bạn vài lần để đảm bảo bạn có thuật ngữ đúng!

Đó là lý do tại sao việc gán lại `studentName` chỉ ảnh hưởng đến biến bên trong (tham số): `studentName` XANH DƯƠNG(2), không phải `studentName` toàn cục ĐỎ(1).

Khi bạn chọn che khuất một biến từ phạm vi bên ngoài, một tác động trực tiếp là từ phạm vi đó vào trong/xuống (thông qua bất kỳ phạm vi lồng nhau nào) bây giờ không thể nào cho bất kỳ viên bi nào được tô màu như biến bị che khuất—(ĐỎ(1), trong trường hợp này). Nói cách khác, bất kỳ tham chiếu định danh `studentName` nào sẽ tương ứng với biến tham số đó, không bao giờ là biến toàn cục `studentName`. Về mặt từ vựng, không thể tham chiếu đến `studentName` toàn cục ở bất cứ đâu bên trong hàm `printStudent(..)` (hoặc từ bất kỳ phạm vi lồng nhau nào).

### Thủ Thuật Bỏ Che Khuất Toàn Cục

Xin lưu ý: tận dụng kỹ thuật mà tôi sắp mô tả không phải là thực hành tốt, vì nó hạn chế về tiện ích, gây nhầm lẫn cho người đọc mã của bạn, và có thể mời lỗi vào chương trình của bạn. Tôi chỉ đề cập đến nó vì bạn có thể gặp hành vi này trong các chương trình hiện có, và việc hiểu những gì đang xảy ra là rất quan trọng để không bị vấp ngã.

*Có thể* truy cập một biến toàn cục từ một phạm vi nơi biến đó đã bị che khuất, nhưng không thông qua tham chiếu định danh từ vựng điển hình.

Trong phạm vi toàn cục (ĐỎ(1)), các khai báo `var` và khai báo `function` cũng tự phơi bày như các thuộc tính (có cùng tên với định danh) trên *đối tượng toàn cục*—về cơ bản là một biểu diễn đối tượng của phạm vi toàn cục. Nếu bạn đã viết JS cho môi trường trình duyệt, bạn có thể nhận ra đối tượng toàn cục là `window`. Điều đó không *hoàn toàn* chính xác, nhưng nó đủ tốt cho cuộc thảo luận của chúng ta. Trong chương tiếp theo, chúng ta sẽ khám phá chủ đề phạm vi/đối tượng toàn cục nhiều hơn.

Hãy xem xét chương trình này, được thực thi cụ thể như một tệp .js độc lập trong môi trường trình duyệt:

```js
var studentName = "Suzy";

function printStudent(studentName) {
    console.log(studentName);
    console.log(window.studentName);
}

printStudent("Frank");
// "Frank"
// "Suzy"
```

Chú ý tham chiếu `window.studentName`? Biểu thức này đang truy cập biến toàn cục `studentName` như một thuộc tính trên `window` (mà chúng ta đang giả vờ bây giờ là đồng nghĩa với đối tượng toàn cục). Đó là cách duy nhất để truy cập một biến bị che khuất từ bên trong một phạm vi nơi biến che khuất có mặt.

`window.studentName` là một bản sao của biến toàn cục `studentName`, không phải là một bản sao ảnh chụp riêng biệt. Thay đổi đối với một vẫn được nhìn thấy từ cái kia, theo cả hai hướng. Bạn có thể nghĩ về `window.studentName` như một getter/setter truy cập biến `studentName` thực tế. Trên thực tế, bạn thậm chí có thể *thêm* một biến vào phạm vi toàn cục bằng cách tạo/thiết lập một thuộc tính trên đối tượng toàn cục.

| CẢNH BÁO: |
| :--- |
| Hãy nhớ: chỉ vì bạn *có thể* không có nghĩa là bạn *nên*. Đừng che khuất một biến toàn cục mà bạn cần truy cập, và ngược lại, tránh sử dụng thủ thuật này để truy cập một biến toàn cục mà bạn đã che khuất. Và chắc chắn đừng làm bối rối người đọc mã của bạn bằng cách tạo các biến toàn cục như thuộc tính `window` thay vì với các khai báo chính thức! |

Thủ thuật nhỏ này chỉ hoạt động để truy cập một biến phạm vi toàn cục (không phải biến bị che khuất từ phạm vi lồng nhau), và thậm chí sau đó, chỉ một biến được khai báo với `var` hoặc `function`.

Các dạng khai báo phạm vi toàn cục khác không tạo ra các thuộc tính đối tượng toàn cục được phản chiếu:

```js
var one = 1;
let notOne = 2;
const notTwo = 3;
class notThree {}

console.log(window.one);       // 1
console.log(window.notOne);    // undefined
console.log(window.notTwo);    // undefined
console.log(window.notThree);  // undefined
```

Các biến (bất kể chúng được khai báo như thế nào!) tồn tại trong bất kỳ phạm vi nào khác ngoài phạm vi toàn cục hoàn toàn không thể truy cập được từ một phạm vi nơi chúng đã bị che khuất:

```js
var special = 42;

function lookingFor(special) {
    // Định danh `special` (tham số) trong phạm vi
    // này bị che khuất bên trong keepLooking(), và
    // do đó không thể truy cập được từ phạm vi đó.

    function keepLooking() {
        var special = 3.141592;
        console.log(special);
        console.log(window.special);
    }

    keepLooking();
}

lookingFor(112358132134);
// 3.141592
// 42
```

`special` toàn cục ĐỎ(1) bị che khuất bởi `special` XANH DƯƠNG(2) (tham số), và `special` XANH DƯƠNG(2) tự nó bị che khuất bởi `special` XANH LÁ(3) bên trong `keepLooking()`. Chúng ta vẫn có thể truy cập `special` ĐỎ(1) bằng cách sử dụng tham chiếu gián tiếp `window.special`. Nhưng không có cách nào cho `keepLooking()` truy cập `special` XANH DƯƠNG(2) chứa số `112358132134`.

### Sao Chép Không Phải Là Truy Cập

Tôi đã được hỏi câu hỏi "Nhưng còn...?" sau đây hàng chục lần. Hãy xem xét:

```js
var special = 42;

function lookingFor(special) {
    var another = {
        special: special
    };

    function keepLooking() {
        var special = 3.141592;
        console.log(special);
        console.log(another.special);  // Ồ, tinh vi!
        console.log(window.special);
    }

    keepLooking();
}

lookingFor(112358132134);
// 3.141592
// 112358132134
// 42
```

Ồ! Vậy kỹ thuật đối tượng `another` này có bác bỏ tuyên bố của tôi rằng tham số `special` "hoàn toàn không thể truy cập" từ bên trong `keepLooking()` không? Không, tuyên bố vẫn đúng.

`special: special` đang sao chép giá trị của biến tham số `special` vào một container khác (một thuộc tính có cùng tên). Tất nhiên, nếu bạn đặt một giá trị vào một container khác, shadowing không còn áp dụng (trừ khi `another` cũng bị che khuất!). Nhưng điều đó không có nghĩa là chúng ta đang truy cập tham số `special`; nó có nghĩa là chúng ta đang truy cập bản sao của giá trị mà nó có tại thời điểm đó, bằng cách *container khác* (thuộc tính đối tượng). Chúng ta không thể gán lại tham số `special` XANH DƯƠNG(2) thành một giá trị khác từ bên trong `keepLooking()`.

Một "Nhưng...!?" khác bạn có thể sắp nêu ra: điều gì sẽ xảy ra nếu tôi đã sử dụng các đối tượng hoặc mảng làm giá trị thay vì các số (`112358132134`, v.v.)? Liệu chúng ta có tham chiếu đến các đối tượng thay vì bản sao của các giá trị nguyên thủy "sửa" sự không thể truy cập?

Không. Thay đổi nội dung của giá trị đối tượng thông qua bản sao tham chiếu **không** giống với việc truy cập từ vựng biến chính nó. Chúng ta vẫn không thể gán lại tham số `special` XANH DƯƠNG(2).

### Che Khuất Bất Hợp Pháp

Không phải tất cả các kết hợp của che khuất khai báo đều được phép. `let` có thể che khuất `var`, nhưng `var` không thể che khuất `let`:

```js
function something() {
    var special = "JavaScript";

    {
        let special = 42;   // che khuất hoàn toàn tốt

        // ..
    }
}

function another() {
    // ..

    {
        let special = "JavaScript";

        {
            var special = "JavaScript";
            // ^^^ Syntax Error

            // ..
        }
    }
}
```

Chú ý trong hàm `another()`, khai báo `var special` bên trong đang cố gắng khai báo một `special` toàn hàm, mà bản thân nó là tốt (như được hiển thị bởi hàm `something()`).

Mô tả lỗi cú pháp trong trường hợp này chỉ ra rằng `special` đã được định nghĩa, nhưng thông báo lỗi đó hơi gây hiểu lầm—một lần nữa, không có lỗi như vậy xảy ra trong `something()`, vì shadowing thường được phép tốt.

Lý do thực sự nó được nêu ra như một `SyntaxError` là vì `var` về cơ bản đang cố gắng "vượt qua ranh giới" của (hoặc nhảy qua) khai báo `let` có cùng tên, điều này không được phép.

Lệnh cấm vượt ranh giới đó thực sự dừng lại ở mỗi ranh giới hàm, vì vậy biến thể này không gây ra ngoại lệ:

```js
function another() {
    // ..

    {
        let special = "JavaScript";

        ajax("https://some.url",function callback(){
            // che khuất hoàn toàn tốt
            var special = "JavaScript";

            // ..
        });
    }
}
```

Tóm tắt: `let` (trong phạm vi bên trong) luôn có thể che khuất `var` của phạm vi bên ngoài. `var` (trong phạm vi bên trong) chỉ có thể che khuất `let` của phạm vi bên ngoài nếu có ranh giới hàm ở giữa.

## Phạm Vi Tên Hàm

Như bạn đã thấy cho đến nay, một khai báo `function` trông như thế này:

```js
function askQuestion() {
    // ..
}
```

Và như đã thảo luận trong Chương 1 và 2, một khai báo `function` như vậy sẽ tạo một định danh trong phạm vi bao quanh (trong trường hợp này, phạm vi toàn cục) có tên `askQuestion`.

Còn chương trình này thì sao?

```js
var askQuestion = function(){
    // ..
};
```

Điều tương tự cũng đúng cho biến `askQuestion` đang được tạo. Nhưng vì đó là một `function` expression—một định nghĩa hàm được sử dụng như giá trị thay vì một khai báo độc lập—bản thân hàm sẽ không "hoist" (xem Chương 5).

Một sự khác biệt lớn giữa khai báo `function` và biểu thức `function` là những gì xảy ra với định danh tên của hàm. Hãy xem xét một biểu thức `function` được đặt tên:

```js
var askQuestion = function ofTheTeacher(){
    // ..
};
```

Chúng ta biết `askQuestion` kết thúc trong phạm vi bên ngoài. Nhưng còn định danh `ofTheTeacher` thì sao? Đối với khai báo `function` chính thức, định danh tên kết thúc trong phạm vi bên ngoài/bao quanh, vì vậy có thể hợp lý khi giả định đó là trường hợp ở đây. Nhưng `ofTheTeacher` được khai báo là một định danh **bên trong chính hàm**:

```js
var askQuestion = function ofTheTeacher() {
    console.log(ofTheTeacher);
};

askQuestion();
// function ofTheTeacher()...

console.log(ofTheTeacher);
// ReferenceError: ofTheTeacher is not defined
```

| LƯU Ý: |
| :--- |
| Thực ra, `ofTheTeacher` không chính xác *trong phạm vi của hàm*. Phụ lục A, "Phạm Vi Ngầm Định" sẽ giải thích thêm. |

Không chỉ `ofTheTeacher` được khai báo bên trong hàm thay vì bên ngoài, mà nó cũng được định nghĩa là chỉ đọc:

```js
var askQuestion = function ofTheTeacher() {
    "use strict";
    ofTheTeacher = 42;   // TypeError

    //..
};

askQuestion();
// TypeError
```

Bởi vì chúng ta đã sử dụng chế độ nghiêm ngặt, thất bại gán được báo cáo là một `TypeError`; trong chế độ không nghiêm ngặt, một phép gán như vậy thất bại âm thầm mà không có ngoại lệ.

Còn khi một biểu thức `function` không có định danh tên thì sao?

```js
var askQuestion = function(){
   // ..
};
```

Một biểu thức `function` với một định danh tên được gọi là "biểu thức hàm được đặt tên", nhưng một biểu thức không có định danh tên được gọi là "biểu thức hàm ẩn danh". Các biểu thức hàm ẩn danh rõ ràng không có định danh tên ảnh hưởng đến phạm vi nào.

| LƯU Ý: |
| :--- |
| Chúng ta sẽ thảo luận về các biểu thức `function` được đặt tên so với ẩn danh chi tiết hơn nhiều, bao gồm các yếu tố ảnh hưởng đến quyết định sử dụng cái này hay cái kia, trong Phụ lục A. |

## Hàm Mũi Tên

ES6 đã thêm một dạng biểu thức `function` bổ sung vào ngôn ngữ, được gọi là "hàm mũi tên":

```js
var askQuestion = () => {
    // ..
};
```

Hàm mũi tên `=>` không yêu cầu từ `function` để định nghĩa nó. Ngoài ra, `( .. )` xung quanh danh sách tham số là tùy chọn trong một số trường hợp đơn giản. Tương tự, `{ .. }` xung quanh thân hàm là tùy chọn trong một số trường hợp. Và khi `{ .. }` bị bỏ qua, một giá trị trả về được gửi ra mà không cần sử dụng từ khóa `return`.

| LƯU Ý: |
| :--- |
| Sức hấp dẫn của hàm mũi tên `=>` thường được bán như "cú pháp ngắn hơn", và điều đó được tuyên bố là tương đương với mã có thể đọc được khách quan hơn. Tuyên bố này là đáng ngờ nhất, và tôi tin rằng hoàn toàn bị sai lầm. Chúng ta sẽ đào sâu vào "khả năng đọc" của các dạng hàm khác nhau trong Phụ lục A. |

Các hàm mũi tên là ẩn danh từ vựng, có nghĩa là chúng không có định danh liên quan trực tiếp tham chiếu đến hàm. Phép gán cho `askQuestion` tạo ra một tên suy luận là "askQuestion", nhưng đó **không giống với việc không ẩn danh**:

```js
var askQuestion = () => {
    // ..
};

askQuestion.name;   // askQuestion
```

Các hàm mũi tên đạt được sự ngắn gọn cú pháp của chúng với chi phí phải tâm trí xử lý một loạt các biến thể cho các dạng/điều kiện khác nhau. Chỉ một vài, ví dụ:

```js
() => 42;

id => id.toUpperCase();

(id,name) => ({ id, name });

(...args) => {
    return args[args.length - 1];
};
```

Lý do thực sự tôi đưa ra các hàm mũi tên là vì tuyên bố phổ biến nhưng không chính xác rằng các hàm mũi tên bằng cách nào đó hoạt động khác nhau liên quan đến phạm vi từ vựng so với các hàm `function` tiêu chuẩn.

Điều này không chính xác.

Ngoài việc là ẩn danh (và không có dạng khai báo), các hàm mũi tên `=>` có cùng quy tắc phạm vi từ vựng như các hàm `function`. Một hàm mũi tên, có hoặc không có `{ .. }` xung quanh thân của nó, vẫn tạo ra một xô phạm vi lồng nhau, bên trong riêng biệt. Các khai báo biến bên trong xô phạm vi lồng nhau này hoạt động giống như trong phạm vi `function`.

## Lùi Lại

Khi một hàm (khai báo hoặc biểu thức) được định nghĩa, một phạm vi mới được tạo ra. Vị trí của các phạm vi lồng bên trong nhau tạo ra một hệ thống phân cấp phạm vi tự nhiên trong toàn bộ chương trình, được gọi là chuỗi phạm vi. Chuỗi phạm vi kiểm soát truy cập biến, được định hướng lên và ra ngoài.

Mỗi phạm vi mới cung cấp một bảng sạch, một không gian để giữ tập hợp biến riêng của nó. Khi một tên biến được lặp lại ở các cấp độ khác nhau của chuỗi phạm vi, shadowing xảy ra, ngăn chặn truy cập vào biến bên ngoài từ điểm đó vào trong.

Khi chúng ta lùi lại từ những chi tiết tinh tế hơn này, chương tiếp theo chuyển trọng tâm sang phạm vi chính mà tất cả các chương trình JS bao gồm: phạm vi toàn cục.
