# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Chương 2: Minh Họa Phạm Vi Từ Vựng

Trong Chương 1, chúng ta đã khám phá cách phạm vi được xác định trong quá trình biên dịch mã, một mô hình được gọi là "phạm vi từ vựng". Thuật ngữ "từ vựng" đề cập đến giai đoạn đầu tiên của biên dịch (lexing/parsing).

Để *suy luận* đúng về các chương trình của chúng ta, điều quan trọng là phải có một nền tảng khái niệm vững chắc về cách phạm vi hoạt động. Nếu chúng ta dựa vào phỏng đoán và trực giác, chúng ta có thể vô tình nhận được câu trả lời đúng một số lần, nhưng nhiều lần khác chúng ta lại sai lầm. Đây không phải là công thức cho thành công.

Giống như hồi lớp toán ở trường tiểu học, việc có câu trả lời đúng là chưa đủ nếu chúng ta không chỉ ra các bước chính xác để đạt được nó! Chúng ta cần xây dựng các mô hình tinh thần chính xác và hữu ích làm nền tảng để tiến về phía trước.

Chương này sẽ minh họa *phạm vi* với một số phép ẩn dụ. Mục tiêu ở đây là *suy nghĩ* về cách chương trình của bạn được xử lý bởi công cụ JS theo những cách gần gũi hơn với cách công cụ JS thực sự hoạt động.

## Bi, Xô và Bong Bóng... Trời Ơi!

Một phép ẩn dụ mà tôi thấy hiệu quả trong việc hiểu phạm vi là phân loại các viên bi màu vào các xô có màu phù hợp.

Hãy tưởng tượng bạn bắt gặp một đống bi, và nhận thấy rằng tất cả các viên bi đều có màu đỏ, xanh dương hoặc xanh lá. Hãy phân loại tất cả các viên bi, thả những viên đỏ vào xô đỏ, xanh lá vào xô xanh lá, và xanh dương vào xô xanh dương. Sau khi phân loại, khi bạn sau này cần một viên bi xanh lá, bạn đã biết xô xanh lá là nơi để lấy nó.

Trong phép ẩn dụ này, các viên bi là các biến trong chương trình của chúng ta. Các xô là các phạm vi (hàm và khối), mà chúng ta chỉ gán màu riêng lẻ theo khái niệm cho mục đích thảo luận của chúng ta. Màu của mỗi viên bi do đó được xác định bởi phạm vi *màu* nào mà chúng ta tìm thấy viên bi ban đầu được tạo ra.

Hãy chú thích ví dụ chương trình đang chạy từ Chương 1 với các nhãn màu phạm vi:

```js
// phạm vi ngoài cùng/toàn cục: ĐỎ

var students = [
    { id: 14, name: "Kyle" },
    { id: 73, name: "Suzy" },
    { id: 112, name: "Frank" },
    { id: 6, name: "Sarah" }
];

function getStudentName(studentID) {
    // phạm vi hàm: XANH DƯƠNG

    for (let student of students) {
        // phạm vi vòng lặp: XANH LÁ

        if (student.id == studentID) {
            return student.name;
        }
    }
}

var nextStudent = getStudentName(73);
console.log(nextStudent);   // Suzy
```

Chúng ta đã chỉ định ba màu phạm vi với các nhận xét mã: ĐỎ (phạm vi toàn cục ngoài cùng), XANH DƯƠNG (phạm vi của hàm `getStudentName(..)`), và XANH LÁ (phạm vi của/bên trong vòng lặp `for`). Nhưng vẫn có thể khó nhận ra ranh giới của các xô phạm vi này khi nhìn vào danh sách mã.

Hình 2 giúp hình dung các ranh giới của các phạm vi bằng cách vẽ các bong bóng màu (hay còn gọi là xô) xung quanh mỗi phạm vi:

<figure>
    <img src="images/fig2.png" width="500" alt="Colored Scope Bubbles" align="center">
    <figcaption><em>Hình 2: Bong Bóng Phạm Vi Màu</em></figcaption>
</figure>

1. **Bong bóng 1** (ĐỎ) bao gồm phạm vi toàn cục, chứa ba định danh/biến: `students` (dòng 1), `getStudentName` (dòng 8), và `nextStudent` (dòng 16).

2. **Bong bóng 2** (XANH DƯƠNG) bao gồm phạm vi của hàm `getStudentName(..)` (dòng 8), chỉ chứa một định danh/biến: tham số `studentID` (dòng 8).

3. **Bong bóng 3** (XANH LÁ) bao gồm phạm vi của vòng lặp `for` (dòng 9), chỉ chứa một định danh/biến: `student` (dòng 9).

| LƯU Ý: |
| :--- |
| Về mặt kỹ thuật, tham số `studentID` không chính xác trong phạm vi XANH DƯƠNG(2). Chúng ta sẽ làm rõ sự nhầm lẫn đó trong "Phạm Vi Ngầm Định" trong Phụ lục A. Hiện tại, gần đúng khi gắn nhãn `studentID` là viên bi XANH DƯƠNG(2). |

Các bong bóng phạm vi được xác định trong quá trình biên dịch dựa trên nơi các hàm/khối phạm vi được viết, sự lồng nhau bên trong nhau, v.v. Mỗi bong bóng phạm vi hoàn toàn được chứa trong bong bóng phạm vi cha của nó—một phạm vi không bao giờ một phần trong hai phạm vi bên ngoài khác nhau.

Mỗi viên bi (biến/định danh) được tô màu dựa trên bong bóng (xô) nào nó được khai báo, không phải màu của phạm vi mà nó có thể được truy cập (ví dụ: `students` trên dòng 9 và `studentID` trên dòng 10).

| LƯU Ý: |
| :--- |
| Hãy nhớ chúng ta đã khẳng định trong Chương 1 rằng `id`, `name`, và `log` đều là thuộc tính, không phải biến; nói cách khác, chúng không phải là viên bi trong xô, vì vậy chúng không được tô màu dựa trên bất kỳ quy tắc nào mà chúng ta đang thảo luận trong cuốn sách này. Để hiểu cách các truy cập thuộc tính như vậy được xử lý, hãy xem cuốn sách thứ ba trong bộ, *Objects & Classes*. |

Khi công cụ JS xử lý một chương trình (trong quá trình biên dịch), và tìm thấy một khai báo cho một biến, về cơ bản nó hỏi, "Tôi hiện đang ở trong phạm vi *màu* nào (bong bóng hoặc xô)?" Biến được chỉ định là cùng *màu* đó, có nghĩa là nó thuộc về xô/bong bóng đó.

Xô XANH LÁ(3) hoàn toàn lồng bên trong xô XANH DƯƠNG(2), và tương tự xô XANH DƯƠNG(2) hoàn toàn lồng bên trong xô ĐỎ(1). Các phạm vi có thể lồng nhau như được hiển thị, đến bất kỳ độ sâu lồng nhau nào mà chương trình của bạn cần.

Các tham chiếu (không phải khai báo) đến biến/định danh được phép nếu có một khai báo khớp trong phạm vi hiện tại, hoặc bất kỳ phạm vi nào ở trên/bên ngoài phạm vi hiện tại, nhưng không với các khai báo từ các phạm vi thấp hơn/lồng nhau.

Một biểu thức trong xô ĐỎ(1) chỉ có quyền truy cập vào các viên bi ĐỎ(1), **không phải** XANH DƯƠNG(2) hoặc XANH LÁ(3). Một biểu thức trong xô XANH DƯƠNG(2) có thể tham chiếu đến các viên bi XANH DƯƠNG(2) hoặc ĐỎ(1), **không phải** XANH LÁ(3). Và một biểu thức trong xô XANH LÁ(3) có quyền truy cập vào các viên bi ĐỎ(1), XANH DƯƠNG(2), và XANH LÁ(3).

Chúng ta có thể khái niệm hóa quá trình xác định các màu viên bi không phải khai báo này trong thời gian chạy như một tra cứu. Vì tham chiếu biến `students` trong câu lệnh vòng lặp `for` trên dòng 9 không phải là một khai báo, nó không có màu. Vì vậy, chúng ta hỏi xô phạm vi XANH DƯƠNG(2) hiện tại nếu nó có một viên bi khớp với tên đó. Vì nó không có, tra cứu tiếp tục với phạm vi bên ngoài/chứa tiếp theo: ĐỎ(1). Xô ĐỎ(1) có một viên bi có tên `students`, vì vậy tham chiếu biến `students` của câu lệnh vòng lặp được xác định là viên bi ĐỎ(1).

Câu lệnh `if (student.id == studentID)` trên dòng 10 tương tự được xác định tham chiếu đến một viên bi XANH LÁ(3) có tên `student` và một viên bi XANH DƯƠNG(2) `studentID`.

| LƯU Ý: |
| :--- |
| Công cụ JS thường không xác định các màu viên bi này trong thời gian chạy; "tra cứu" ở đây là một thiết bị tu từ để giúp bạn hiểu các khái niệm. Trong quá trình biên dịch, hầu hết hoặc tất cả các tham chiếu biến sẽ khớp với các xô phạm vi đã biết, vì vậy màu của chúng đã được xác định, và được lưu trữ với mỗi tham chiếu viên bi để tránh tra cứu không cần thiết khi chương trình chạy. Thêm về sắc thái này trong Chương 3. |

Các điểm chính từ bi & xô (và bong bóng!):

* Các biến được khai báo trong các phạm vi cụ thể, có thể được coi như các viên bi màu từ các xô có màu phù hợp.

* Bất kỳ tham chiếu biến nào xuất hiện trong phạm vi nơi nó được khai báo, hoặc xuất hiện trong bất kỳ phạm vi lồng sâu hơn nào, sẽ được gắn nhãn là viên bi có cùng màu đó—trừ khi một phạm vi can thiệp "che khuất" khai báo biến; xem "Shadowing" trong Chương 3.

* Việc xác định các xô màu, và các viên bi mà chúng chứa, xảy ra trong quá trình biên dịch. Thông tin này được sử dụng cho "tra cứu" biến (màu viên bi) trong quá trình thực thi mã.

## Cuộc Trò Chuyện Giữa Các Bạn Bè

Một phép ẩn dụ hữu ích khác cho quá trình phân tích các biến và các phạm vi mà chúng đến từ đó là tưởng tượng các cuộc trò chuyện khác nhau xảy ra bên trong công cụ khi mã được xử lý và sau đó được thực thi. Chúng ta có thể "lắng nghe" các cuộc trò chuyện này để có nền tảng khái niệm tốt hơn về cách các phạm vi hoạt động.

Bây giờ hãy gặp các thành viên của công cụ JS sẽ có các cuộc trò chuyện khi họ xử lý chương trình của chúng ta:

* *Engine* (Công cụ): chịu trách nhiệm biên dịch và thực thi từ đầu đến cuối chương trình JavaScript của chúng ta.

* *Compiler* (Trình biên dịch): một trong những người bạn của *Engine*; xử lý tất cả công việc bẩn của phân tích cú pháp và tạo mã (xem phần trước).

* *Scope Manager* (Trình quản lý phạm vi): một người bạn khác của *Engine*; thu thập và duy trì danh sách tra cứu của tất cả các biến/định danh đã khai báo, và thực thi một tập hợp các quy tắc về cách chúng có thể truy cập được đối với mã đang thực thi hiện tại.

Để bạn *hiểu đầy đủ* cách JavaScript hoạt động, bạn cần bắt đầu *suy nghĩ* như *Engine* (và bạn bè) suy nghĩ, đặt các câu hỏi mà họ đặt, và trả lời các câu hỏi của họ tương tự.

Để khám phá các cuộc trò chuyện này, hãy nhớ lại một lần nữa ví dụ chương trình đang chạy của chúng ta:

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

Hãy kiểm tra cách JS sẽ xử lý chương trình đó, cụ thể bắt đầu với câu lệnh đầu tiên. Mảng và nội dung của nó chỉ là các giá trị literal JS cơ bản (và do đó không bị ảnh hưởng bởi bất kỳ mối quan tâm phạm vi nào), vì vậy trọng tâm của chúng ta ở đây sẽ là các phần khai báo và gán khởi tạo `var students = [ .. ]`.

Chúng ta thường nghĩ về điều đó như một câu lệnh duy nhất, nhưng đó không phải là cách bạn *Engine* của chúng ta nhìn thấy nó. Trên thực tế, JS coi chúng là hai hoạt động riêng biệt, một hoạt động mà *Compiler* sẽ xử lý trong quá trình biên dịch, và hoạt động khác mà *Engine* sẽ xử lý trong quá trình thực thi.

Điều đầu tiên *Compiler* sẽ làm với chương trình này là thực hiện lexing để chia nó thành các token, sau đó nó sẽ phân tích cú pháp thành một cây (AST).

Khi *Compiler* đến tạo mã, có nhiều chi tiết cần xem xét hơn những gì có thể rõ ràng. Một giả định hợp lý sẽ là *Compiler* sẽ tạo mã cho câu lệnh đầu tiên như: "Phân bổ bộ nhớ cho một biến, gắn nhãn nó là `students`, sau đó gắn một tham chiếu đến mảng vào biến đó." Nhưng đó không phải là toàn bộ câu chuyện.

Đây là các bước *Compiler* sẽ làm theo để xử lý câu lệnh đó:

1. Gặp `var students`, *Compiler* sẽ hỏi *Scope Manager* để xem liệu một biến có tên `students` đã tồn tại cho xô phạm vi cụ thể đó chưa. Nếu có, *Compiler* sẽ bỏ qua khai báo này và tiếp tục. Nếu không, *Compiler* sẽ tạo mã mà (tại thời điểm thực thi) yêu cầu *Scope Manager* tạo một biến mới có tên `students` trong xô phạm vi đó.

2. *Compiler* sau đó tạo mã cho *Engine* để thực thi sau này, để xử lý phép gán `students = []`. Mã *Engine* chạy trước tiên sẽ hỏi *Scope Manager* nếu có một biến có tên `students` có thể truy cập được trong xô phạm vi hiện tại. Nếu không, *Engine* tiếp tục tìm kiếm ở nơi khác (xem "Phạm Vi Lồng Nhau" bên dưới). Khi *Engine* tìm thấy một biến, nó gán tham chiếu của mảng `[ .. ]` cho nó.

Ở dạng trò chuyện, giai đoạn đầu tiên của biên dịch cho chương trình có thể diễn ra giữa *Compiler* và *Scope Manager* như thế này:

> ***Compiler***: Này, *Scope Manager* (của phạm vi toàn cục), tôi tìm thấy một khai báo chính thức cho một định danh có tên `students`, bạn đã nghe nói về nó chưa?

> ***(Global) Scope Manager***: Không, chưa bao giờ nghe nói về nó, vì vậy tôi vừa tạo nó cho bạn.

> ***Compiler***: Này, *Scope Manager*, tôi tìm thấy một khai báo chính thức cho một định danh có tên `getStudentName`, bạn đã nghe nói về nó chưa?

> ***(Global) Scope Manager***: Không, nhưng tôi vừa tạo nó cho bạn.

> ***Compiler***: Này, *Scope Manager*, `getStudentName` trỏ đến một hàm, vì vậy chúng ta cần một xô phạm vi mới.

> ***(Function) Scope Manager***: Được rồi, đây là xô phạm vi.

> ***Compiler***: Này, *Scope Manager* (của hàm), tôi tìm thấy một khai báo tham số chính thức cho `studentID`, bạn đã nghe nói về nó chưa?

> ***(Function) Scope Manager***: Không, nhưng bây giờ nó được tạo trong phạm vi này.

> ***Compiler***: Này, *Scope Manager* (của hàm), tôi tìm thấy một vòng lặp `for` sẽ cần xô phạm vi riêng của nó.

> ...

Cuộc trò chuyện là một trao đổi hỏi-đáp, trong đó **Compiler** hỏi *Scope Manager* hiện tại nếu một khai báo định danh gặp phải đã được gặp trước đó. Nếu "không," *Scope Manager* tạo biến đó trong phạm vi đó. Nếu câu trả lời là "có," thì nó thực sự bị bỏ qua vì không có gì khác cho *Scope Manager* đó làm.

*Compiler* cũng báo hiệu khi nó chạy qua các hàm hoặc phạm vi khối, để một xô phạm vi mới và *Scope Manager* có thể được khởi tạo.

Sau này, khi đến thực thi chương trình, cuộc trò chuyện sẽ chuyển sang *Engine* và *Scope Manager*, và có thể diễn ra như thế này:

> ***Engine***: Này, *Scope Manager* (của phạm vi toàn cục), trước khi chúng ta bắt đầu, bạn có thể tra cứu định danh `getStudentName` để tôi có thể gán hàm này cho nó không?

> ***(Global) Scope Manager***: Vâng, đây là biến.

> ***Engine***: Này, *Scope Manager*, tôi tìm thấy một tham chiếu *mục tiêu* cho `students`, bạn đã nghe nói về nó chưa?

> ***(Global) Scope Manager***: Có, nó đã được khai báo chính thức cho phạm vi này, vì vậy đây là nó.

> ***Engine***: Cảm ơn, tôi đang khởi tạo `students` thành `undefined`, vì vậy nó sẵn sàng để sử dụng.

> Này, *Scope Manager* (của phạm vi toàn cục), tôi tìm thấy một tham chiếu *mục tiêu* cho `nextStudent`, bạn đã nghe nói về nó chưa?

> ***(Global) Scope Manager***: Có, nó đã được khai báo chính thức cho phạm vi này, vì vậy đây là nó.

> ***Engine***: Cảm ơn, tôi đang khởi tạo `nextStudent` thành `undefined`, vì vậy nó sẵn sàng để sử dụng.

> Này, *Scope Manager* (của phạm vi toàn cục), tôi tìm thấy một tham chiếu *nguồn* cho `getStudentName`, bạn đã nghe nói về nó chưa?

> ***(Global) Scope Manager***: Có, nó đã được khai báo chính thức cho phạm vi này. Đây là nó.

> ***Engine***: Tuyệt vời, giá trị trong `getStudentName` là một hàm, vì vậy tôi sẽ thực thi nó.

> ***Engine***: Này, *Scope Manager*, bây giờ chúng ta cần khởi tạo phạm vi của hàm.

> ...

Cuộc trò chuyện này là một trao đổi hỏi-đáp khác, trong đó *Engine* trước tiên yêu cầu *Scope Manager* hiện tại tra cứu định danh `getStudentName` được hoisted, để liên kết hàm với nó. *Engine* sau đó tiến hành hỏi *Scope Manager* về tham chiếu *mục tiêu* cho `students`, v.v.

Để xem lại và tóm tắt cách một câu lệnh như `var students = [ .. ]` được xử lý, trong hai bước riêng biệt:

1. *Compiler* thiết lập khai báo của biến phạm vi (vì nó chưa được khai báo trước đó trong phạm vi hiện tại).

2. Trong khi *Engine* đang thực thi, để xử lý phần gán của câu lệnh, *Engine* yêu cầu *Scope Manager* tra cứu biến, khởi tạo nó thành `undefined` để nó sẵn sàng sử dụng, và sau đó gán giá trị mảng cho nó.

## Phạm Vi Lồng Nhau

Khi đến lúc thực thi hàm `getStudentName()`, *Engine* yêu cầu một thể hiện *Scope Manager* cho phạm vi của hàm đó, và sau đó nó sẽ tiến hành tra cứu tham số (`studentID`) để gán giá trị đối số `73` cho nó, v.v.

Phạm vi hàm cho `getStudentName(..)` được lồng bên trong phạm vi toàn cục. Phạm vi khối của vòng lặp `for` tương tự được lồng bên trong phạm vi hàm đó. Các phạm vi có thể được lồng từ vựng đến bất kỳ độ sâu tùy ý nào mà chương trình định nghĩa.

Mỗi phạm vi nhận được thể hiện *Scope Manager* riêng của nó mỗi khi phạm vi đó được thực thi (một hoặc nhiều lần). Mỗi phạm vi tự động có tất cả các định danh của nó được đăng ký ở đầu phạm vi đang được thực thi (điều này được gọi là "variable hoisting"; xem Chương 5).

Ở đầu một phạm vi, nếu bất kỳ định danh nào đến từ khai báo `function`, biến đó được tự động khởi tạo thành tham chiếu hàm liên quan của nó. Và nếu bất kỳ định danh nào đến từ khai báo `var` (trái ngược với `let`/`const`), biến đó được tự động khởi tạo thành `undefined` để nó có thể được sử dụng; nếu không, biến vẫn chưa được khởi tạo (hay còn gọi là, trong "TDZ" của nó, xem Chương 5) và không thể được sử dụng cho đến khi khai báo-và-khởi tạo đầy đủ của nó được thực thi.

Trong câu lệnh `for (let student of students) {`, `students` là một tham chiếu *nguồn* phải được tra cứu. Nhưng tra cứu đó sẽ được xử lý như thế nào, vì phạm vi của hàm sẽ không tìm thấy một định danh như vậy?

Để giải thích, hãy tưởng tượng cuộc trò chuyện đó diễn ra như thế này:

> ***Engine***: Này, *Scope Manager* (cho hàm), tôi có một tham chiếu *nguồn* cho `students`, bạn đã nghe nói về nó chưa?

> ***(Function) Scope Manager***: Không, chưa bao giờ nghe nói về nó. Hãy thử phạm vi bên ngoài tiếp theo.

> ***Engine***: Này, *Scope Manager* (cho phạm vi toàn cục), tôi có một tham chiếu *nguồn* cho `students`, bạn đã nghe nói về nó chưa?

> ***(Global) Scope Manager***: Vâng, nó đã được khai báo chính thức, đây là nó.

> ...

Một trong những khía cạnh chính của phạm vi từ vựng là bất cứ khi nào một tham chiếu định danh không thể được tìm thấy trong phạm vi hiện tại, phạm vi bên ngoài tiếp theo trong lồng nhau được tham khảo; quá trình đó được lặp lại cho đến khi tìm thấy câu trả lời hoặc không còn phạm vi nào để tham khảo.

### Thất Bại Tra Cứu

Khi *Engine* cạn kiệt tất cả các phạm vi *có sẵn từ vựng* (di chuyển ra ngoài) và vẫn không thể giải quyết tra cứu của một định danh, một điều kiện lỗi sau đó tồn tại. Tuy nhiên, tùy thuộc vào chế độ của chương trình (chế độ nghiêm ngặt hay không) và vai trò của biến (tức là *mục tiêu* so với *nguồn*; xem Chương 1), điều kiện lỗi này sẽ được xử lý khác nhau.

#### Mớ Hỗn Độn Undefined

Nếu biến là *nguồn*, một tra cứu định danh không được giải quyết được coi là một biến chưa được khai báo (không biết, thiếu), luôn dẫn đến một `ReferenceError` được ném ra. Ngoài ra, nếu biến là *mục tiêu*, và mã tại thời điểm đó đang chạy trong chế độ nghiêm ngặt, biến được coi là chưa được khai báo và tương tự ném ra một `ReferenceError`.

Thông báo lỗi cho một điều kiện biến chưa được khai báo, trong hầu hết các môi trường JS, sẽ trông giống như, "Reference Error: XYZ is not defined." Cụm từ "not defined" có vẻ gần như giống hệt với từ "undefined", theo ngôn ngữ tiếng Anh. Nhưng hai điều này rất khác nhau trong JS, và thông báo lỗi này thật không may tạo ra một sự nhầm lẫn dai dẳng.

"Not defined" thực sự có nghĩa là "not declared" (không được khai báo)—hoặc, đúng hơn, "undeclared" (chưa được khai báo), như trong một biến không có khai báo chính thức khớp trong bất kỳ phạm vi *có sẵn từ vựng* nào. Ngược lại, "undefined" thực sự có nghĩa là một biến đã được tìm thấy (đã khai báo), nhưng biến nếu không có giá trị khác trong đó vào lúc này, vì vậy nó mặc định là giá trị `undefined`.

Để làm cho sự nhầm lẫn còn lâu dài hơn nữa, toán tử `typeof` của JS trả về chuỗi `"undefined"` cho các tham chiếu biến trong cả hai trạng thái:

```js
var studentName;
typeof studentName;     // "undefined"

typeof doesntExist;     // "undefined"
```

Hai tham chiếu biến này đang ở trong các điều kiện rất khác nhau, nhưng JS chắc chắn làm cho nước bị đục. Mớ hỗn độn thuật ngữ là khó hiểu và vô cùng đáng tiếc. Thật không may, các nhà phát triển JS chỉ phải chú ý kỹ để không nhầm lẫn *loại* "undefined" nào mà họ đang xử lý!

#### Toàn Cục... Cái Gì!?

Nếu biến là *mục tiêu* và chế độ nghiêm ngặt không có hiệu lực, một hành vi di sản khó hiểu và đáng ngạc nhiên sẽ xảy ra. Kết quả rắc rối là *Scope Manager* của phạm vi toàn cục sẽ chỉ tạo một **biến toàn cục ngẫu nhiên** để thực hiện phép gán mục tiêu đó!

Hãy xem xét:

```js
function getStudentName() {
    // gán cho một biến chưa được khai báo :(
    nextStudent = "Suzy";
}

getStudentName();

console.log(nextStudent);
// "Suzy" -- rất tiếc, một biến toàn cục ngẫu nhiên!
```

Đây là cách *cuộc trò chuyện* đó sẽ tiến hành:

> ***Engine***: Này, *Scope Manager* (cho hàm), tôi có một tham chiếu *mục tiêu* cho `nextStudent`, bạn đã nghe nói về nó chưa?

> ***(Function) Scope Manager***: Không, chưa bao giờ nghe nói về nó. Hãy thử phạm vi bên ngoài tiếp theo.

> ***Engine***: Này, *Scope Manager* (cho phạm vi toàn cục), tôi có một tham chiếu *mục tiêu* cho `nextStudent`, bạn đã nghe nói về nó chưa?

> ***(Global) Scope Manager***: Không, nhưng vì chúng ta đang ở chế độ không nghiêm ngặt, tôi đã giúp bạn và vừa tạo một biến toàn cục cho bạn, đây là nó!

Kinh tởm.

Loại tai nạn này (gần như chắc chắn dẫn đến lỗi cuối cùng) là một ví dụ tuyệt vời về các biện pháp bảo vệ có lợi được cung cấp bởi chế độ nghiêm ngặt, và tại sao đó là một ý tưởng tồi tệ *không* sử dụng chế độ nghiêm ngặt. Trong chế độ nghiêm ngặt, ***Global Scope Manager*** thay vào đó sẽ phản hồi:

> ***(Global) Scope Manager***: Không, chưa bao giờ nghe nói về nó. Xin lỗi, tôi phải ném một `ReferenceError`.

Gán cho một biến chưa bao giờ được khai báo *là* một lỗi, vì vậy đúng là chúng ta sẽ nhận được một `ReferenceError` ở đây.

Không bao giờ dựa vào các biến toàn cục ngẫu nhiên. Luôn sử dụng chế độ nghiêm ngặt, và luôn khai báo chính thức các biến của bạn. Sau đó, bạn sẽ nhận được một `ReferenceError` hữu ích nếu bạn từng nhầm lẫn cố gắng gán cho một biến chưa được khai báo.

### Xây Dựng Trên Các Phép Ẩn Dụ

Để hình dung giải quyết phạm vi lồng nhau, tôi thích một phép ẩn dụ khác nữa, một tòa nhà văn phòng, như trong Hình 3:

<figure>
    <img src="images/fig3.png" width="250" alt="Scope &quot;Building&quot;" align="center">
    <figcaption><em>Hình 3: "Tòa Nhà" Phạm Vi</em></figcaption>
    <br><br>
</figure>

Tòa nhà đại diện cho bộ sưu tập phạm vi lồng nhau của chương trình của chúng ta. Tầng đầu tiên của tòa nhà đại diện cho phạm vi đang thực thi hiện tại. Tầng cao nhất của tòa nhà là phạm vi toàn cục.

Bạn giải quyết một tham chiếu biến *mục tiêu* hoặc *nguồn* bằng cách trước tiên nhìn vào tầng hiện tại, và nếu bạn không tìm thấy nó, đi thang máy lên tầng tiếp theo (tức là một phạm vi bên ngoài), nhìn ở đó, sau đó là tầng tiếp theo, v.v. Khi bạn đến tầng cao nhất (phạm vi toàn cục), bạn hoặc tìm thấy những gì bạn đang tìm kiếm, hoặc bạn không. Nhưng bạn phải dừng lại bất kể.

## Tiếp Tục Cuộc Trò Chuyện

Đến thời điểm này, bạn nên đang phát triển các mô hình tinh thần phong phú hơn về phạm vi là gì và cách công cụ JS xác định và sử dụng nó từ mã của bạn.

Trước khi *tiếp tục*, hãy tìm một số mã trong một trong các dự án của bạn và chạy qua các cuộc trò chuyện này. Nghiêm túc, thực sự nói to lên. Tìm một người bạn và thực hành từng vai trò với họ. Nếu một trong hai bạn thấy mình bối rối hoặc vấp ngã, hãy dành nhiều thời gian hơn để xem lại tài liệu này.

Khi chúng ta di chuyển (lên) đến chương tiếp theo (bên ngoài), chúng ta sẽ khám phá cách các phạm vi từ vựng của một chương trình được kết nối trong một chuỗi.
