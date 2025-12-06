---
layout: default
title: Chương 4
parent: Phạm Vi & Closures
nav_order: 5
---

# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Chương 4: Xung Quanh Phạm Vi Toàn Cục

Chương 3 đã đề cập đến "phạm vi toàn cục" nhiều lần, nhưng bạn vẫn có thể tự hỏi tại sao phạm vi ngoài cùng của một chương trình lại quan trọng đến vậy trong JS hiện đại. Phần lớn công việc hiện nay được thực hiện bên trong các hàm và module thay vì toàn cục.

Liệu có đủ tốt để chỉ khẳng định, "Tránh sử dụng phạm vi toàn cục," và xong việc?

Phạm vi toàn cục của một chương trình JS là một chủ đề phong phú, với nhiều tiện ích và sắc thái hơn bạn có thể cho là. Chương này đầu tiên khám phá cách phạm vi toàn cục (vẫn) hữu ích và liên quan đến việc viết các chương trình JS ngày nay, sau đó xem xét sự khác biệt về nơi và *cách truy cập* phạm vi toàn cục trong các môi trường JS khác nhau.

Hiểu đầy đủ phạm vi toàn cục là rất quan trọng trong việc làm chủ việc sử dụng phạm vi từ vựng để cấu trúc các chương trình của bạn.

## Tại Sao Phạm Vi Toàn Cục?

Có lẽ không có gì ngạc nhiên với độc giả rằng hầu hết các ứng dụng được tạo thành từ nhiều (đôi khi rất nhiều!) tệp JS riêng lẻ. Vậy chính xác tất cả các tệp riêng biệt đó được ghép lại với nhau trong một ngữ cảnh thời gian chạy duy nhất bởi công cụ JS như thế nào?

Đối với các ứng dụng được thực thi trên trình duyệt, có ba cách chính.

Đầu tiên, nếu bạn đang sử dụng trực tiếp các module ES (không chuyển đổi chúng thành một số định dạng gói module khác), các tệp này được tải riêng lẻ bởi môi trường JS. Sau đó, mỗi module `import` tham chiếu đến bất kỳ module nào khác mà nó cần truy cập. Các tệp module riêng biệt hợp tác với nhau độc quyền thông qua các import được chia sẻ này, mà không cần bất kỳ phạm vi bên ngoài được chia sẻ nào.

Thứ hai, nếu bạn đang sử dụng một bundler trong quá trình xây dựng của mình, tất cả các tệp thường được nối với nhau trước khi gửi đến trình duyệt và công cụ JS, sau đó chỉ xử lý một tệp lớn. Ngay cả khi tất cả các phần của ứng dụng được đặt cùng nhau trong một tệp duy nhất, một số cơ chế là cần thiết để mỗi phần đăng ký một *tên* để được tham chiếu bởi các phần khác, cũng như một số cơ sở để truy cập đó xảy ra.

Trong một số thiết lập xây dựng, toàn bộ nội dung của tệp được bao bọc trong một phạm vi bao quanh duy nhất, chẳng hạn như một hàm bao bọc, module phổ quát (UMD—xem Phụ lục A), v.v. Mỗi phần có thể đăng ký chính nó để truy cập từ các phần khác bằng cách các biến cục bộ trong phạm vi được chia sẻ đó. Ví dụ:

```js
(function wrappingOuterScope(){
    var moduleOne = (function one(){
        // ..
    })();

    var moduleTwo = (function two(){
        // ..

        function callModuleOne() {
            moduleOne.someMethod();
        }

        // ..
    })();
})();
```

Như được hiển thị, các biến cục bộ `moduleOne` và `moduleTwo` bên trong phạm vi hàm `wrappingOuterScope()` được khai báo để các module này có thể truy cập lẫn nhau để hợp tác.

Mặc dù phạm vi của `wrappingOuterScope()` là một hàm chứ không phải phạm vi toàn cục môi trường đầy đủ, nó hoạt động như một loại "phạm vi toàn ứng dụng", một xô nơi tất cả các định danh cấp cao nhất có thể được lưu trữ, mặc dù không phải trong phạm vi toàn cục thực sự. Nó giống như một người thay thế cho phạm vi toàn cục trong khía cạnh đó.

Và cuối cùng, cách thứ ba: cho dù một công cụ bundler được sử dụng cho một ứng dụng, hoặc cho dù các tệp (không phải module ES) chỉ đơn giản được tải trong trình duyệt riêng lẻ (thông qua thẻ `<script>` hoặc tải tài nguyên JS động khác), nếu không có phạm vi bao quanh duy nhất bao gồm tất cả các phần này, **phạm vi toàn cục** là cách duy nhất để chúng hợp tác với nhau:

Một tệp được gói loại này thường trông giống như thế này:

```js
var moduleOne = (function one(){
    // ..
})();
var moduleTwo = (function two(){
    // ..

    function callModuleOne() {
        moduleOne.someMethod();
    }

    // ..
})();
```

Ở đây, vì không có phạm vi hàm bao quanh, các khai báo `moduleOne` và `moduleTwo` này chỉ đơn giản được thả vào phạm vi toàn cục. Điều này thực sự giống như thể các tệp không được nối, mà được tải riêng biệt.

Ngoài việc (có khả năng) giải thích cho nơi mã của ứng dụng nằm trong thời gian chạy, và cách mỗi phần có thể truy cập các phần khác để hợp tác, phạm vi toàn cục cũng là nơi:

* JS phơi bày các built-in của nó:
    - primitives: `undefined`, `null`, `Infinity`, `NaN`
    - natives: `Date()`, `Object()`, `String()`, v.v.
    - global functions: `eval()`, `parseInt()`, v.v.
    - namespaces: `Math`, `Atomics`, `JSON`
    - friends of JS: `Intl`, `WebAssembly`

* Môi trường lưu trữ công cụ JS phơi bày các built-in riêng của nó:
    - `console` (và các phương thức của nó)
    - DOM (`window`, `document`, v.v.)
    - timers (`setTimeout(..)`, v.v.)
    - web platform APIs: `navigator`, `history`, geolocation, WebRTC, v.v.

Đây chỉ là một số trong nhiều *globals* mà các chương trình của bạn sẽ tương tác.

| LƯU Ý: |
| :--- |
| Node cũng phơi bày một số phần tử "toàn cục", nhưng về mặt kỹ thuật chúng không nằm trong phạm vi `global`: `require()`, `__dirname`, `module`, `URL`, v.v. |

Hầu hết các nhà phát triển đồng ý rằng phạm vi toàn cục không nên chỉ là một bãi rác cho mọi biến trong ứng dụng của bạn. Đó là một mớ hỗn độn của các lỗi chỉ chờ đợi để xảy ra. Nhưng cũng không thể phủ nhận rằng phạm vi toàn cục là một *keo dán* quan trọng cho hầu như mọi ứng dụng JS.

## Chính Xác Phạm Vi Toàn Cục Này Ở Đâu?

Có vẻ rõ ràng rằng phạm vi toàn cục nằm ở phần ngoài cùng của một tệp; tức là, không bên trong bất kỳ hàm hoặc khối nào khác. Nhưng nó không hoàn toàn đơn giản như vậy.

Các môi trường JS khác nhau xử lý các phạm vi của các chương trình của bạn, đặc biệt là phạm vi toàn cục, khác nhau. Khá phổ biến đối với các nhà phát triển JS có những quan niệm sai lầm mà thậm chí không nhận ra.

### "Window" Trình Duyệt

Đối với việc xử lý phạm vi toàn cục, môi trường *thuần khiết* nhất mà JS có thể chạy là như một tệp .js độc lập được tải trong môi trường trang web trong trình duyệt. Tôi không có nghĩa là "thuần khiết" như trong không có gì được tự động thêm vào—rất nhiều có thể được thêm vào!—mà là về mặt xâm nhập tối thiểu vào mã hoặc can thiệp với hành vi phạm vi toàn cục mong đợi của nó.

Hãy xem xét tệp .js này:

```js
var studentName = "Kyle";

function hello() {
    console.log(`Hello, ${ studentName }!`);
}

hello();
// Hello, Kyle!
```

Mã này có thể được tải trong môi trường trang web bằng thẻ `<script>` nội tuyến, thẻ script `<script src=..>` trong đánh dấu, hoặc thậm chí một phần tử DOM `<script>` được tạo động. Trong cả ba trường hợp, các định danh `studentName` và `hello` được khai báo trong phạm vi toàn cục.

Điều đó có nghĩa là nếu bạn truy cập đối tượng toàn cục (thường là `window` trong trình duyệt), bạn sẽ tìm thấy các thuộc tính có cùng tên ở đó:

```js
var studentName = "Kyle";

function hello() {
    console.log(`Hello, ${ window.studentName }!`);
}

window.hello();
// Hello, Kyle!
```

Đó là hành vi mặc định mà người ta mong đợi từ việc đọc đặc tả JS: phạm vi bên ngoài *là* phạm vi toàn cục và `studentName` được tạo hợp pháp như biến toàn cục.

Đó là những gì tôi có nghĩa là *thuần khiết*. Nhưng thật không may, điều đó sẽ không phải lúc nào cũng đúng với tất cả các môi trường JS bạn gặp, và điều đó thường đáng ngạc nhiên đối với các nhà phát triển JS.

#### Toàn Cục Che Khuất Toàn Cục

Nhớ lại cuộc thảo luận về shadowing (và global unshadowing) từ Chương 3, nơi một khai báo biến có thể ghi đè và ngăn chặn truy cập vào một khai báo có cùng tên từ phạm vi bên ngoài.

Một hậu quả bất thường của sự khác biệt giữa một biến toàn cục và một thuộc tính toàn cục có cùng tên là, chỉ trong phạm vi toàn cục, một thuộc tính đối tượng toàn cục có thể bị che khuất bởi một biến toàn cục:

```js
window.something = 42;

let something = "Kyle";

console.log(something);
// Kyle

console.log(window.something);
// 42
```

Khai báo `let` thêm một biến toàn cục `something` nhưng không phải là một thuộc tính đối tượng toàn cục (xem Chương 3). Hiệu ứng sau đó là định danh từ vựng `something` che khuất thuộc tính đối tượng toàn cục `something`.

Gần như chắc chắn là một ý tưởng tồi để tạo ra sự khác biệt giữa đối tượng toàn cục và phạm vi toàn cục. Người đọc mã của bạn gần như chắc chắn sẽ bị vấp ngã.

Một cách đơn giản để tránh gotcha này với các khai báo toàn cục: luôn sử dụng `var` cho globals. Dành `let` và `const` cho phạm vi khối (xem "Phạm Vi với Khối" trong Chương 6).

### Web Workers

Web Workers là một phần mở rộng nền tảng web trên đỉnh hành vi browser-JS, cho phép một tệp JS chạy trong một luồng hoàn toàn riêng biệt (theo hệ điều hành) từ luồng đang chạy chương trình JS chính.

Vì các chương trình Web Worker này chạy trên một luồng riêng biệt, chúng bị hạn chế trong giao tiếp của chúng với luồng ứng dụng chính, để tránh/giới hạn các điều kiện race và các biến chứng khác. Mã Web Worker không có quyền truy cập vào DOM, ví dụ. Tuy nhiên, một số web API được cung cấp cho worker, chẳng hạn như `navigator`.

Vì Web Worker được coi là một chương trình hoàn toàn riêng biệt, nó không chia sẻ phạm vi toàn cục với chương trình JS chính. Tuy nhiên, công cụ JS của trình duyệt vẫn đang chạy mã, vì vậy chúng ta có thể mong đợi *sự thuần khiết* tương tự của hành vi phạm vi toàn cục của nó. Vì không có quyền truy cập DOM, bí danh `window` cho phạm vi toàn cục không tồn tại.

Trong Web Worker, tham chiếu đối tượng toàn cục thường được thực hiện bằng cách sử dụng `self`:

```js
var studentName = "Kyle";
let studentID = 42;

function hello() {
    console.log(`Hello, ${ self.studentName }!`);
}

self.hello();
// Hello, Kyle!

self.studentID;
// undefined
```

Giống như với các chương trình JS chính, các khai báo `var` và `function` tạo ra các thuộc tính được phản chiếu trên đối tượng toàn cục (hay còn gọi là `self`), trong khi các khai báo khác (`let`, v.v.) thì không.

### ES Modules (ESM)

ES6 đã giới thiệu hỗ trợ hạng nhất cho mẫu module (được đề cập trong Chương 8). Một trong những tác động rõ ràng nhất của việc sử dụng ESM là cách nó thay đổi hành vi của phạm vi cấp cao nhất có thể quan sát được trong một tệp.

Nhớ lại đoạn mã này từ trước đó (mà chúng ta sẽ điều chỉnh sang định dạng ESM bằng cách sử dụng từ khóa `export`):

```js
var studentName = "Kyle";

function hello() {
    console.log(`Hello, ${ studentName }!`);
}

hello();
// Hello, Kyle!

export hello;
```

Nếu mã đó nằm trong một tệp được tải như một module ES, nó vẫn sẽ chạy chính xác như vậy. Tuy nhiên, các hiệu ứng có thể quan sát được, từ quan điểm ứng dụng tổng thể, sẽ khác.

Mặc dù được khai báo ở cấp cao nhất của tệp (module), trong phạm vi rõ ràng ngoài cùng, `studentName` và `hello` không phải là biến toàn cục. Thay vào đó, chúng là toàn module, hoặc nếu bạn thích, "module-global".

Tuy nhiên, trong một module không có "đối tượng phạm vi toàn module" ngầm định cho các khai báo cấp cao nhất này được thêm vào như thuộc tính, như có khi các khai báo xuất hiện ở cấp cao nhất của các tệp JS không phải module. Điều này không có nghĩa là các biến toàn cục không thể tồn tại hoặc được truy cập trong các chương trình như vậy. Chỉ là các biến toàn cục không được *tạo* bằng cách khai báo các biến trong phạm vi cấp cao nhất của một module.

Phạm vi cấp cao nhất của module được kế thừa từ phạm vi toàn cục, gần như thể toàn bộ nội dung của module được bao bọc trong một hàm. Do đó, tất cả các biến tồn tại trong phạm vi toàn cục (cho dù chúng có trên đối tượng toàn cục hay không!) đều có sẵn như các định danh từ vựng từ bên trong phạm vi của module.

ESM khuyến khích giảm thiểu sự phụ thuộc vào phạm vi toàn cục, nơi bạn import bất kỳ module nào bạn có thể cần cho module hiện tại để hoạt động. Như vậy, bạn ít thấy việc sử dụng phạm vi toàn cục hoặc đối tượng toàn cục của nó hơn.

### Node

Một khía cạnh của Node thường bắt các nhà phát triển JS không chuẩn bị là Node coi mọi tệp .js đơn lẻ mà nó tải, bao gồm cả tệp chính bạn bắt đầu quá trình Node, như một *module* (module ES hoặc module CommonJS, xem Chương 8). Hiệu ứng thực tế là cấp cao nhất của các chương trình Node của bạn **không bao giờ thực sự là phạm vi toàn cục**, theo cách nó khi tải một tệp không phải module trong trình duyệt.

Tại thời điểm viết bài này, Node gần đây đã thêm hỗ trợ cho các module ES. Nhưng ngoài ra, Node đã từ đầu hỗ trợ một định dạng module được gọi là "CommonJS", trông như thế này:

```js
var studentName = "Kyle";

function hello() {
    console.log(`Hello, ${ studentName }!`);
}

hello();
// Hello, Kyle!

module.exports.hello = hello;
```

Trước khi xử lý, Node thực sự bao bọc mã như vậy trong một hàm, để các khai báo `var` và `function` được chứa trong phạm vi hàm bao bọc đó, **không** được coi là biến toàn cục.

Hãy tưởng tượng mã trước đó được Node nhìn thấy như thế này (minh họa, không thực tế):

```js
function Module(module,require,__dirname,...) {
    var studentName = "Kyle";

    function hello() {
        console.log(`Hello, ${ studentName }!`);
    }

    hello();
    // Hello, Kyle!

    module.exports.hello = hello;
}
```

Node sau đó về cơ bản gọi hàm `Module(..)` được thêm vào để chạy module của bạn. Bạn có thể thấy rõ ở đây tại sao các định danh `studentName` và `hello` không phải là toàn cục, mà được khai báo trong phạm vi module.

Như đã lưu ý trước đó, Node định nghĩa một số "globals" như `require()`, nhưng chúng không thực sự là các định danh trong phạm vi toàn cục (cũng không phải là thuộc tính của đối tượng toàn cục). Chúng được tiêm vào phạm vi của mọi module, về cơ bản giống như các tham số được liệt kê trong khai báo hàm `Module(..)`.

Vậy làm thế nào để bạn định nghĩa các biến toàn cục thực sự trong Node? Cách duy nhất để làm như vậy là thêm các thuộc tính vào một "global" khác được Node tự động cung cấp, mà một cách trớ trêu được gọi là `global`. `global` là một tham chiếu đến đối tượng phạm vi toàn cục thực sự, hơi giống như sử dụng `window` trong môi trường JS trình duyệt.

Hãy xem xét:

```js
global.studentName = "Kyle";

function hello() {
    console.log(`Hello, ${ studentName }!`);
}

hello();
// Hello, Kyle!

module.exports.hello = hello;
```

Ở đây chúng ta thêm `studentName` như một thuộc tính trên đối tượng `global`, và sau đó trong câu lệnh `console.log(..)` chúng ta có thể truy cập `studentName` như một biến toàn cục bình thường.

Hãy nhớ rằng, định danh `global` không được định nghĩa bởi JS; nó được định nghĩa cụ thể bởi Node.

## Global This

Xem xét các môi trường JS mà chúng ta đã xem xét cho đến nay, một chương trình có thể hoặc không thể:

* Khai báo một biến toàn cục trong phạm vi cấp cao nhất với các khai báo `var` hoặc `function`—hoặc `let`, `const`, và `class`.

* Cũng thêm các khai báo biến toàn cục như thuộc tính của đối tượng phạm vi toàn cục nếu `var` hoặc `function` được sử dụng cho khai báo.

* Tham chiếu đến đối tượng phạm vi toàn cục (để thêm hoặc truy xuất các biến toàn cục, như thuộc tính) với `window`, `self`, hoặc `global`.

Tôi nghĩ rằng công bằng khi nói rằng truy cập và hành vi phạm vi toàn cục phức tạp hơn hầu hết các nhà phát triển cho là, như các phần trước đã minh họa. Nhưng sự phức tạp không bao giờ rõ ràng hơn khi cố gắng xác định một tham chiếu có thể áp dụng phổ quát cho đối tượng phạm vi toàn cục.

Một "thủ thuật" khác để có được một tham chiếu đến đối tượng phạm vi toàn cục trông như thế này:

```js
const theGlobalScopeObject =
    (new Function("return this"))();
```

| LƯU Ý: |
| :--- |
| Một hàm có thể được xây dựng động từ mã được lưu trữ trong một giá trị chuỗi với constructor `Function()`, tương tự như `eval(..)` (xem "Gian Lận: Sửa Đổi Phạm Vi Thời Gian Chạy" trong Chương 1). Một hàm như vậy sẽ tự động được chạy trong chế độ không nghiêm ngặt (vì lý do di sản) khi được gọi với lệnh gọi hàm `()` bình thường như được hiển thị; `this` của nó sẽ trỏ đến đối tượng toàn cục. Xem cuốn sách thứ ba trong bộ, *Objects & Classes*, để biết thêm thông tin về xác định ràng buộc `this`. |

Vì vậy, chúng ta có `window`, `self`, `global`, và thủ thuật xấu xí `new Function(..)` này. Đó là rất nhiều cách khác nhau để cố gắng đạt được đối tượng toàn cục này. Mỗi cái có ưu và nhược điểm của nó.

Tại sao không giới thiệu thêm một cái nữa!?!?

Kể từ ES2020, JS cuối cùng đã định nghĩa một tham chiếu tiêu chuẩn hóa cho đối tượng phạm vi toàn cục, được gọi là `globalThis`. Vì vậy, tùy thuộc vào tính gần đây của các công cụ JS mà mã của bạn chạy trong đó, bạn có thể sử dụng `globalThis` thay cho bất kỳ cách tiếp cận nào trong số đó.

Chúng ta thậm chí có thể cố gắng định nghĩa một polyfill đa môi trường an toàn hơn trên các môi trường JS trước `globalThis`, chẳng hạn như:

```js
const theGlobalScopeObject =
    (typeof globalThis != "undefined") ? globalThis :
    (typeof global != "undefined") ? global :
    (typeof window != "undefined") ? window :
    (typeof self != "undefined") ? self :
    (new Function("return this"))();
```

Phew! Điều đó chắc chắn không lý tưởng, nhưng nó hoạt động nếu bạn thấy mình cần một tham chiếu phạm vi toàn cục đáng tin cậy.

## Nhận Thức Toàn Cục

Phạm vi toàn cục có mặt và liên quan trong mọi chương trình JS, mặc dù các mẫu hiện đại để tổ chức mã thành các module giảm nhấn mạnh nhiều sự phụ thuộc vào việc lưu trữ các định danh trong không gian tên đó.

Tuy nhiên, khi mã của chúng ta lan rộng ngày càng nhiều ra ngoài giới hạn của trình duyệt, đặc biệt quan trọng là chúng ta có một sự nắm bắt vững chắc về sự khác biệt trong cách phạm vi toàn cục (và đối tượng phạm vi toàn cục!) hoạt động trên các môi trường JS khác nhau.

Với bức tranh toàn cảnh của phạm vi toàn cục bây giờ rõ nét hơn trong tiêu điểm, chương tiếp theo một lần nữa đi sâu vào các chi tiết sâu hơn của phạm vi từ vựng, kiểm tra cách và khi nào các biến có thể được sử dụng.
