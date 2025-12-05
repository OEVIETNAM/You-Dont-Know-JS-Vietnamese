# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Chương 8: Mẫu Module

Trong chương này, chúng ta kết thúc văn bản chính của cuốn sách bằng cách khám phá một trong những mẫu tổ chức mã quan trọng nhất trong tất cả các chương trình: module. Như chúng ta sẽ thấy, các module vốn được xây dựng từ những gì chúng ta đã đề cập: phần thưởng cho những nỗ lực của bạn trong việc học phạm vi từ vựng và closure.

Chúng ta đã kiểm tra mọi góc độ của phạm vi từ vựng, từ bề rộng của phạm vi toàn cục xuống qua các phạm vi khối lồng nhau, vào sự phức tạp của vòng đời biến. Sau đó, chúng ta đã tận dụng phạm vi từ vựng để hiểu toàn bộ sức mạnh của closure.

Hãy dành một chút thời gian để suy ngẫm về việc bạn đã đi bao xa trong hành trình này cho đến nay; bạn đã thực hiện những bước lớn trong việc tìm hiểu sâu hơn về JS!

Chủ đề trung tâm của cuốn sách này là hiểu và làm chủ phạm vi và closure là chìa khóa trong việc cấu trúc và tổ chức mã của chúng ta đúng cách, đặc biệt là các quyết định về nơi lưu trữ thông tin trong các biến.

Mục tiêu của chúng ta trong chương cuối cùng này là đánh giá cao cách các module thể hiện tầm quan trọng của các chủ đề này, nâng chúng từ các khái niệm trừu tượng lên các cải tiến cụ thể, thực tế trong việc xây dựng các chương trình.

## Đóng Gói và Phơi Bày Tối Thiểu (POLE)

Đóng gói (Encapsulation) thường được trích dẫn như một nguyên tắc của lập trình hướng đối tượng (OO), nhưng nó cơ bản và áp dụng rộng rãi hơn thế. Mục tiêu của đóng gói là bó hoặc đồng vị trí thông tin (dữ liệu) và hành vi (hàm) cùng phục vụ một mục đích chung.

Độc lập với bất kỳ cú pháp hoặc cơ chế mã nào, tinh thần của đóng gói có thể được thực hiện trong một cái gì đó đơn giản như sử dụng các tệp riêng biệt để giữ các bit của chương trình tổng thể với mục đích chung. Nếu chúng ta bó mọi thứ cung cấp năng lượng cho một danh sách các kết quả tìm kiếm vào một tệp duy nhất gọi là "search-list.js", chúng ta đang đóng gói phần đó của chương trình.

Xu hướng gần đây trong lập trình front-end hiện đại để tổ chức các ứng dụng xung quanh kiến trúc Component đẩy đóng gói đi xa hơn nữa. Đối với nhiều người, cảm thấy tự nhiên khi hợp nhất mọi thứ cấu thành danh sách kết quả tìm kiếm—thậm chí vượt ra ngoài mã, bao gồm đánh dấu trình bày và kiểu dáng—vào một đơn vị logic chương trình duy nhất, một cái gì đó hữu hình mà chúng ta có thể tương tác. Và sau đó chúng ta dán nhãn bộ sưu tập đó là thành phần "SearchList".

Một mục tiêu chính khác là kiểm soát khả năng hiển thị của các khía cạnh nhất định của dữ liệu và chức năng được đóng gói. Nhớ lại từ Chương 6 nguyên tắc *phơi bày tối thiểu* (POLE), tìm cách bảo vệ một cách phòng thủ chống lại các *nguy hiểm* khác nhau của việc phơi bày quá mức phạm vi; những điều này ảnh hưởng đến cả biến và hàm. Trong JS, chúng ta thường thực hiện kiểm soát khả năng hiển thị thông qua cơ chế của phạm vi từ vựng.

Ý tưởng là nhóm các bit chương trình giống nhau lại với nhau, và giới hạn quyền truy cập lập trình một cách chọn lọc vào các phần chúng ta coi là chi tiết *riêng tư*. Những gì không được coi là *riêng tư* sau đó được đánh dấu là *công khai*, có thể truy cập được cho toàn bộ chương trình.

Hiệu ứng tự nhiên của nỗ lực này là tổ chức mã tốt hơn. Dễ dàng hơn để xây dựng và bảo trì phần mềm khi chúng ta biết mọi thứ ở đâu, với các ranh giới và điểm kết nối rõ ràng và hiển nhiên. Cũng dễ dàng hơn để duy trì chất lượng nếu chúng ta tránh những cạm bẫy của dữ liệu và chức năng bị phơi bày quá mức.

Đây là một số lợi ích chính của việc tổ chức các chương trình JS thành các module.

## Module Là Gì?

Một module là một tập hợp các dữ liệu và hàm liên quan (thường được gọi là phương thức trong bối cảnh này), được đặc trưng bởi sự phân chia giữa các chi tiết *riêng tư* ẩn và các chi tiết *công khai* có thể truy cập, thường được gọi là "API công khai".

Một module cũng có trạng thái: nó duy trì một số thông tin theo thời gian, cùng với chức năng để truy cập và cập nhật thông tin đó.

| LƯU Ý: |
| :--- |
| Một mối quan tâm rộng hơn của mẫu module là hoàn toàn nắm lấy mô-đun hóa cấp hệ thống thông qua liên kết lỏng lẻo và các kỹ thuật kiến trúc chương trình khác. Đó là một chủ đề phức tạp vượt xa giới hạn thảo luận của chúng ta, nhưng đáng để nghiên cứu thêm ngoài cuốn sách này. |

Để có cảm giác tốt hơn về module là gì, hãy so sánh một số đặc điểm module với các mẫu mã hữu ích không hoàn toàn là module.

### Không Gian Tên (Nhóm Không Trạng Thái)

Nếu bạn nhóm một tập hợp các hàm liên quan lại với nhau, không có dữ liệu, thì bạn không thực sự có đóng gói mong đợi mà một module ngụ ý. Thuật ngữ tốt hơn cho nhóm các hàm *không trạng thái* này là một không gian tên (namespace):

```js
// không gian tên, không phải module
var Utils = {
    cancelEvt(evt) {
        evt.preventDefault();
        evt.stopPropagation();
        evt.stopImmediatePropagation();
    },
    wait(ms) {
        return new Promise(function c(res){
            setTimeout(res,ms);
        });
    },
    isValidEmail(email) {
        return /[^@]+@[^@.]+\.[^@.]+/.test(email);
    }
};
```

`Utils` ở đây là một bộ sưu tập hữu ích các tiện ích, nhưng tất cả chúng đều là các hàm độc lập với trạng thái. Tập hợp chức năng lại với nhau nói chung là thực hành tốt, nhưng điều đó không làm cho cái này trở thành một module. Thay vào đó, chúng ta đã định nghĩa một không gian tên `Utils` và tổ chức các hàm dưới nó.

### Cấu Trúc Dữ Liệu (Nhóm Có Trạng Thái)

Ngay cả khi bạn bó dữ liệu và các hàm có trạng thái lại với nhau, nếu bạn không giới hạn khả năng hiển thị của bất kỳ cái nào trong số đó, thì bạn đang dừng lại ở khía cạnh POLE của đóng gói; không đặc biệt hữu ích khi dán nhãn đó là một module.

Hãy xem xét:

```js
// cấu trúc dữ liệu, không phải module
var Student = {
    records: [
        { id: 14, name: "Kyle", grade: 86 },
        { id: 73, name: "Suzy", grade: 87 },
        { id: 112, name: "Frank", grade: 75 },
        { id: 6, name: "Sarah", grade: 91 }
    ],
    getName(studentID) {
        var student = this.records.find(
            student => student.id == studentID
        );
        return student.name;
    }
};

Student.getName(73);
// Suzy
```

Vì `records` là dữ liệu có thể truy cập công khai, không ẩn sau một API công khai, `Student` ở đây không thực sự là một module.

`Student` có khía cạnh dữ liệu-và-chức-năng của đóng gói, nhưng không có khía cạnh kiểm soát khả năng hiển thị. Tốt nhất là dán nhãn cái này là một thể hiện của một cấu trúc dữ liệu.

### Modules (Kiểm Soát Truy Cập Có Trạng Thái)

Để thể hiện tinh thần đầy đủ của mẫu module, chúng ta không chỉ cần nhóm và trạng thái, mà còn cần kiểm soát truy cập thông qua khả năng hiển thị (riêng tư so với công khai).

Hãy biến `Student` từ phần trước thành một module. Chúng ta sẽ bắt đầu với một hình thức tôi gọi là "module cổ điển," ban đầu được gọi là "module tiết lộ" (revealing module) khi nó xuất hiện lần đầu vào đầu những năm 2000. Hãy xem xét:

```js
var Student = (function defineStudent(){
    var records = [
        { id: 14, name: "Kyle", grade: 86 },
        { id: 73, name: "Suzy", grade: 87 },
        { id: 112, name: "Frank", grade: 75 },
        { id: 6, name: "Sarah", grade: 91 }
    ];

    var publicAPI = {
        getName
    };

    return publicAPI;

    // ************************

    function getName(studentID) {
        var student = records.find(
            student => student.id == studentID
        );
        return student.name;
    }
})();

Student.getName(73);   // Suzy
```

`Student` bây giờ là một thể hiện của một module. Nó có một API công khai với một phương thức duy nhất: `getName(..)`. Phương thức này có thể truy cập dữ liệu `records` ẩn riêng tư.

| CẢNH BÁO: |
| :--- |
| Tôi nên chỉ ra rằng dữ liệu sinh viên rõ ràng được mã hóa cứng vào định nghĩa module này chỉ dành cho mục đích minh họa của chúng ta. Một module điển hình trong chương trình của bạn sẽ nhận dữ liệu này từ một nguồn bên ngoài, thường được tải từ cơ sở dữ liệu, tệp dữ liệu JSON, cuộc gọi Ajax, v.v. Dữ liệu sau đó được tiêm vào thể hiện module thường thông qua (các) phương thức trên API công khai của module. |

Định dạng module cổ điển hoạt động như thế nào?

Chú ý rằng thể hiện của module được tạo ra bởi IIFE `defineStudent()` đang được thực thi. IIFE này trả về một đối tượng (có tên `publicAPI`) có một thuộc tính trên đó tham chiếu đến hàm `getName(..)` bên trong.

Đặt tên đối tượng là `publicAPI` là sở thích phong cách của tôi. Đối tượng có thể được đặt tên bất cứ điều gì bạn thích (JS không quan tâm), hoặc bạn có thể chỉ cần trả về một đối tượng trực tiếp mà không cần gán nó cho bất kỳ biến được đặt tên nội bộ nào. Thêm về lựa chọn này trong Phụ lục A.

Từ bên ngoài, `Student.getName(..)` gọi hàm bên trong được phơi bày này, duy trì quyền truy cập vào biến `records` bên trong thông qua closure.

Bạn không *phải* trả về một đối tượng với một hàm là một trong các thuộc tính của nó. Bạn có thể chỉ cần trả về một hàm trực tiếp, thay cho đối tượng. Điều đó vẫn thỏa mãn tất cả các bit cốt lõi của một module cổ điển.

Nhờ cách phạm vi từ vựng hoạt động, việc định nghĩa các biến và hàm bên trong hàm định nghĩa module bên ngoài của bạn làm cho mọi thứ *theo mặc định* là riêng tư. Chỉ các thuộc tính được thêm vào đối tượng API công khai được trả về từ hàm mới được xuất khẩu để sử dụng công khai bên ngoài.

Việc sử dụng một IIFE ngụ ý rằng chương trình của chúng ta chỉ bao giờ cần một thể hiện trung tâm duy nhất của module, thường được gọi là một "singleton." Thật vậy, ví dụ cụ thể này đủ đơn giản để không có lý do rõ ràng nào chúng ta cần bất cứ điều gì hơn chỉ một thể hiện của module `Student`.

#### Nhà Máy Module (Nhiều Thể Hiện)

Nhưng nếu chúng ta muốn định nghĩa một module hỗ trợ nhiều thể hiện trong chương trình của mình, chúng ta có thể điều chỉnh mã một chút:

```js
// hàm nhà máy, không phải singleton IIFE
function defineStudent() {
    var records = [
        { id: 14, name: "Kyle", grade: 86 },
        { id: 73, name: "Suzy", grade: 87 },
        { id: 112, name: "Frank", grade: 75 },
        { id: 6, name: "Sarah", grade: 91 }
    ];

    var publicAPI = {
        getName
    };

    return publicAPI;

    // ************************

    function getName(studentID) {
        var student = records.find(
            student => student.id == studentID
        );
        return student.name;
    }
}

var fullTime = defineStudent();
fullTime.getName(73);            // Suzy
```

Thay vì chỉ định `defineStudent()` là một IIFE, chúng ta chỉ định nghĩa nó như một hàm độc lập bình thường, thường được gọi trong bối cảnh này là một hàm "nhà máy module" (module factory).

Sau đó, chúng ta gọi nhà máy module, tạo ra một thể hiện của module mà chúng ta dán nhãn `fullTime`. Thể hiện module này ngụ ý một thể hiện mới của phạm vi bên trong, và do đó một closure mới mà `getName(..)` giữ trên `records`. `fullTime.getName(..)` bây giờ gọi phương thức trên thể hiện cụ thể đó.

#### Định Nghĩa Module Cổ Điển

Vì vậy, để làm rõ những gì làm cho một cái gì đó trở thành một module cổ điển:

* Phải có một phạm vi bên ngoài, thường là từ một hàm nhà máy module chạy ít nhất một lần.

* Phạm vi bên trong của module phải có ít nhất một phần thông tin ẩn đại diện cho trạng thái cho module.

* Module phải trả về trên API công khai của nó một tham chiếu đến ít nhất một hàm có closure trên trạng thái module ẩn (để trạng thái này thực sự được bảo tồn).

Bạn có thể sẽ gặp các biến thể khác trên cách tiếp cận module cổ điển này, chúng ta sẽ xem xét chi tiết hơn trong Phụ lục A.

## Node CommonJS Modules

Trong Chương 4, chúng ta đã giới thiệu định dạng module CommonJS được sử dụng bởi Node. Không giống như định dạng module cổ điển được mô tả trước đó, nơi bạn có thể bó nhà máy module hoặc IIFE cùng với bất kỳ mã nào khác bao gồm các module khác, các module CommonJS dựa trên tệp; một module mỗi tệp.

Hãy điều chỉnh ví dụ module của chúng ta để tuân thủ định dạng đó:

```js
module.exports.getName = getName;

// ************************

var records = [
    { id: 14, name: "Kyle", grade: 86 },
    { id: 73, name: "Suzy", grade: 87 },
    { id: 112, name: "Frank", grade: 75 },
    { id: 6, name: "Sarah", grade: 91 }
];

function getName(studentID) {
    var student = records.find(
        student => student.id == studentID
    );
    return student.name;
}
```

Các định danh `records` và `getName` nằm trong phạm vi cấp cao nhất của module này, nhưng đó không phải là phạm vi toàn cục (như đã giải thích trong Chương 4). Như vậy, mọi thứ ở đây là *theo mặc định* riêng tư đối với module.

Để phơi bày một cái gì đó trên API công khai của một module CommonJS, bạn thêm một thuộc tính vào đối tượng trống được cung cấp dưới dạng `module.exports`. Trong một số mã di sản cũ hơn, bạn có thể gặp các tham chiếu đến chỉ một `exports` trần trụi, nhưng để rõ ràng mã, bạn nên luôn luôn định danh đầy đủ tham chiếu đó với tiền tố `module.`.

Đối với mục đích phong cách, tôi thích đặt "exports" của mình ở đầu và triển khai module của mình ở dưới cùng. Nhưng các xuất khẩu này có thể được đặt ở bất cứ đâu. Tôi thực sự khuyên bạn nên thu thập tất cả chúng lại với nhau, hoặc ở đầu hoặc cuối tệp của bạn.

Một số nhà phát triển có thói quen thay thế đối tượng xuất khẩu mặc định, như thế này:

```js
// định nghĩa một đối tượng mới cho API
module.exports = {
    // ..exports..
};
```

Có một số điều kỳ quặc với cách tiếp cận này, bao gồm hành vi không mong đợi nếu nhiều module như vậy phụ thuộc vòng tròn vào nhau. Như vậy, tôi khuyên không nên thay thế đối tượng. Nếu bạn muốn gán nhiều xuất khẩu cùng một lúc, sử dụng định nghĩa kiểu literal đối tượng, bạn có thể làm điều này thay thế:

```js
Object.assign(module.exports,{
   // .. exports ..
});
```

Những gì đang xảy ra ở đây là định nghĩa literal đối tượng `{ .. }` với API công khai của module của bạn được chỉ định, và sau đó `Object.assign(..)` đang thực hiện một bản sao nông của tất cả các thuộc tính đó vào đối tượng `module.exports` hiện có, thay vì thay thế nó. Đây là một sự cân bằng tốt đẹp của sự tiện lợi và hành vi module an toàn hơn.

Để bao gồm một thể hiện module khác vào module/chương trình của bạn, hãy sử dụng phương thức `require(..)` của Node. Giả sử module này nằm tại "/path/to/student.js", đây là cách chúng ta có thể truy cập nó:

```js
var Student = require("/path/to/student.js");

Student.getName(73);
// Suzy
```

`Student` bây giờ tham chiếu đến API công khai của module ví dụ của chúng ta.

Các module CommonJS hoạt động như các thể hiện singleton, tương tự như kiểu định nghĩa module IIFE được trình bày trước đó. Bất kể bạn `require(..)` cùng một module bao nhiêu lần, bạn chỉ nhận được các tham chiếu bổ sung đến thể hiện module được chia sẻ duy nhất.

`require(..)` là một cơ chế tất cả hoặc không có gì; nó bao gồm một tham chiếu của toàn bộ API công khai được phơi bày của module. Để truy cập hiệu quả chỉ một phần của API, cách tiếp cận điển hình trông như thế này:

```js
var getName = require("/path/to/student.js").getName;

// hoặc thay thế:

var { getName } = require("/path/to/student.js");
```

Tương tự như định dạng module cổ điển, các phương thức được xuất khẩu công khai của API của một module CommonJS giữ các closure trên các chi tiết module bên trong. Đó là cách trạng thái singleton module được duy trì trong suốt vòng đời của chương trình của bạn.

| LƯU Ý: |
| :--- |
| Trong các câu lệnh `require("student")` của Node, các đường dẫn không tuyệt đối (`"student"`) giả định phần mở rộng tệp ".js" và tìm kiếm "node_modules". |

## Modern ES Modules (ESM)

Định dạng ESM chia sẻ một số điểm tương đồng với định dạng CommonJS. ESM dựa trên tệp, và các thể hiện module là singleton, với mọi thứ riêng tư *theo mặc định*. Một sự khác biệt đáng chú ý là các tệp ESM được giả định là chế độ nghiêm ngặt (strict-mode), mà không cần một chỉ thị `"use strict"` ở đầu. Không có cách nào để định nghĩa một ESM là không chế độ nghiêm ngặt.

Thay vì `module.exports` trong CommonJS, ESM sử dụng từ khóa `export` để phơi bày một cái gì đó trên API công khai của module. Từ khóa `import` thay thế câu lệnh `require(..)`. Hãy điều chỉnh "students.js" để sử dụng định dạng ESM:

```js
export { getName };

// ************************

var records = [
    { id: 14, name: "Kyle", grade: 86 },
    { id: 73, name: "Suzy", grade: 87 },
    { id: 112, name: "Frank", grade: 75 },
    { id: 6, name: "Sarah", grade: 91 }
];

function getName(studentID) {
    var student = records.find(
        student => student.id == studentID
    );
    return student.name;
}
```

Thay đổi duy nhất ở đây là câu lệnh `export { getName }`. Như trước đây, các câu lệnh `export` có thể xuất hiện ở bất cứ đâu trong tệp, mặc dù `export` phải ở phạm vi cấp cao nhất; nó không thể ở bên trong bất kỳ khối hoặc hàm nào khác.

ESM cung cấp một chút biến thể về cách các câu lệnh `export` có thể được chỉ định. Ví dụ:

```js
export function getName(studentID) {
    // ..
}
```

Mặc dù `export` xuất hiện trước từ khóa `function` ở đây, hình thức này vẫn là một khai báo `function` cũng tình cờ được xuất khẩu. Nghĩa là, định danh `getName` được *hoisted hàm* (xem Chương 5), vì vậy nó có sẵn trong toàn bộ phạm vi của module.

Một biến thể được phép khác:

```js
export default function getName(studentID) {
    // ..
}
```

Đây là cái gọi là "xuất khẩu mặc định" (default export), có ngữ nghĩa khác với các xuất khẩu khác. Về bản chất, một "xuất khẩu mặc định" là một cách viết tắt cho người tiêu dùng của module khi họ `import`, cung cấp cho họ một cú pháp ngắn gọn hơn khi họ chỉ cần thành viên API mặc định duy nhất này.

Các xuất khẩu không phải `default` được gọi là "xuất khẩu được đặt tên" (named exports).

Từ khóa `import`—giống như `export`, nó phải được sử dụng chỉ ở cấp cao nhất của một ESM bên ngoài bất kỳ khối hoặc hàm nào—cũng có một số biến thể trong cú pháp. Đầu tiên được gọi là "nhập khẩu được đặt tên" (named import):

```js
import { getName } from "/path/to/students.js";

getName(73);   // Suzy
```

Như bạn có thể thấy, hình thức này chỉ nhập các thành viên API công khai được đặt tên cụ thể từ một module (bỏ qua bất cứ thứ gì không được đặt tên rõ ràng), và nó thêm các định danh đó vào phạm vi cấp cao nhất của module hiện tại. Loại nhập khẩu này là một phong cách quen thuộc với những người đã quen với nhập khẩu gói trong các ngôn ngữ như Java.

Nhiều thành viên API có thể được liệt kê bên trong tập hợp `{ .. }`, được phân tách bằng dấu phẩy. Một nhập khẩu được đặt tên cũng có thể được *đổi tên* với từ khóa `as`:

```js
import { getName as getStudentName }
   from "/path/to/students.js";

getStudentName(73);
// Suzy
```

Nếu `getName` là một "xuất khẩu mặc định" của module, chúng ta có thể nhập nó như thế này:

```js
import getName from "/path/to/students.js";

getName(73);   // Suzy
```

Sự khác biệt duy nhất ở đây là bỏ `{ }` xung quanh ràng buộc nhập khẩu. Nếu bạn muốn trộn một nhập khẩu mặc định với các nhập khẩu được đặt tên khác:

```js
import { default as getName, /* .. khác .. */ }
   from "/path/to/students.js";

getName(73);   // Suzy
```

Ngược lại, biến thể chính khác trên `import` được gọi là "nhập khẩu không gian tên" (namespace import):

```js
import * as Student from "/path/to/students.js";

Student.getName(73);   // Suzy
```

Như có thể thấy rõ, `*` nhập tất cả mọi thứ được xuất khẩu sang API, mặc định và được đặt tên, và lưu trữ tất cả dưới định danh không gian tên duy nhất như được chỉ định. Cách tiếp cận này phù hợp nhất với hình thức của các module cổ điển trong hầu hết lịch sử của JS.

| LƯU Ý: |
| :--- |
| Tại thời điểm viết bài này, các trình duyệt hiện đại đã hỗ trợ ESM trong vài năm nay, nhưng hỗ trợ ổn định của Node cho ESM là khá gần đây, và đã phát triển trong một thời gian khá dài. Sự phát triển có khả năng tiếp tục trong một năm hoặc hơn nữa; việc giới thiệu ESM vào JS trở lại trong ES6 đã tạo ra một số lo ngại về khả năng tương thích đầy thách thức cho khả năng tương tác của Node với các module CommonJS. Tham khảo tài liệu ESM của Node để biết tất cả các chi tiết mới nhất: https://nodejs.org/api/esm.html |

## Thoát Phạm Vi

Cho dù bạn sử dụng định dạng module cổ điển (trình duyệt hoặc Node), định dạng CommonJS (trong Node), hoặc định dạng ESM (trình duyệt hoặc Node), các module là một trong những cách hiệu quả nhất để cấu trúc và tổ chức chức năng và dữ liệu của chương trình của bạn.

Mẫu module là kết luận của hành trình của chúng ta trong cuốn sách này về việc học cách chúng ta có thể sử dụng các quy tắc của phạm vi từ vựng để đặt các biến và hàm ở các vị trí thích hợp. POLE là tư thế phòng thủ *riêng tư theo mặc định* mà chúng ta luôn thực hiện, đảm bảo chúng ta tránh phơi bày quá mức và chỉ tương tác với diện tích bề mặt API công khai tối thiểu cần thiết.

Và bên dưới các module, *phép thuật* về cách tất cả trạng thái module của chúng ta được duy trì là các closure tận dụng hệ thống phạm vi từ vựng.

Đó là tất cả cho văn bản chính. Chúc mừng bạn đã có một hành trình khá dài cho đến nay! Như tôi đã nói nhiều lần trong suốt, đó là một ý tưởng thực sự tốt để tạm dừng, suy ngẫm, và thực hành những gì chúng ta vừa thảo luận.

Khi bạn thoải mái và sẵn sàng, hãy xem các phụ lục, đào sâu hơn vào một số góc của các chủ đề này, và cũng thách thức bạn với một số bài tập thực hành để củng cố những gì bạn đã học.
