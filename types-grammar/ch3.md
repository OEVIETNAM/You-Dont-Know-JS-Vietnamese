# You Don't Know JS Yet: Các Kiểu & Ngữ Pháp - Ấn bản thứ 2

# Chương 3: Các Giá Trị Đối Tượng

| LƯU Ý: |
| :--- |
| Đang thực hiện |

Bây giờ chúng ta đã thoải mái với các kiểu giá trị nguyên thủy tích hợp sẵn, chúng ta chuyển sự chú ý sang các kiểu `object` (đối tượng) trong JS.

Tôi có thể viết cả một cuốn sách nói sâu về các đối tượng; thực tế, tôi đã làm rồi! Tiêu đề "Đối Tượng & Lớp" (Objects & Classes) của loạt sách này đã bao gồm các đối tượng một cách chuyên sâu, vì vậy hãy chắc chắn rằng bạn đã đọc nó trước khi tiếp tục với chương này.

Thay vì lặp lại nội dung của cuốn sách đó, ở đây chúng ta sẽ tập trung sự chú ý vào cách loại giá trị `object` hoạt động và tương tác với các giá trị khác trong JS.

## Các Kiểu Đối Tượng

Kiểu giá trị `object` bao gồm một vài kiểu phụ, mỗi kiểu có các hành vi chuyên biệt, bao gồm:

* đối tượng thuần (plain objects)
* đối tượng cơ bản (fundamental objects) (nguyên thủy đóng hộp - boxed primitives)
* đối tượng tích hợp (built-in objects)
* mảng (arrays)
* biểu thức chính quy (regular expressions)
* hàm (functions) (hay còn gọi là "đối tượng có thể gọi" - "callable objects")

Ngoài các hành vi chuyên biệt, một đặc điểm chung là tất cả các đối tượng có thể hoạt động như các tập hợp (của các thuộc tính) chứa các giá trị (bao gồm cả hàm/phương thức).

## Các Đối Tượng Thuần (Plain Objects)

Kiểu giá trị đối tượng chung đôi khi được gọi là *đối tượng javascript thuần* (plain ol' javascript objects - POJOs).

Các đối tượng thuần có dạng literal:

```js
address = {
    street: "12345 Market St",
    city: "San Francisco",
    state: "CA",
    zip: "94114"
};
```

Đối tượng thuần này (POJO), như được định nghĩa với dấu ngoặc nhọn `{ .. }`, là một tập hợp các thuộc tính được đặt tên (`street`, `city`, `state`, và `zip`). Các thuộc tính có thể chứa bất kỳ giá trị nào, nguyên thủy hoặc các đối tượng khác (bao gồm mảng, hàm, v.v.).

Cùng một đối tượng cũng có thể được định nghĩa một cách mệnh lệnh bằng cách sử dụng hàm tạo `new Object()`:

```js
address = new Object();
address.street = "12345 Market St";
address.city = "San Francisco";
address.state = "CA";
address.zip = "94114";
```

Các đối tượng thuần theo mặc định được liên kết `[[Prototype]]` với `Object.prototype`, cung cấp cho chúng quyền truy cập được ủy quyền tới một vài phương thức đối tượng chung, chẳng hạn như:

* `toString()` / `toLocaleString()`
* `valueOf()`
* `isPrototypeOf(..)`
* `hasOwnProperty(..)` (gần đây đã bị phản đối -- thay thế: tiện ích tĩnh `Object.hasOwn(..)`)
* `propertyIsEnumerable(..)`
* `__proto__` (hàm getter)

```js
address.isPrototypeOf(Object.prototype);    // true
address.isPrototypeOf({});                  // false
```

## Các Đối Tượng Cơ Bản (Fundamental Objects)

JS xác định một vài kiểu đối tượng *cơ bản*, là các thể hiện của các hàm tạo tích hợp khác nhau, bao gồm:

* `new String()`
* `new Number()`
* `new Boolean()`

Lưu ý rằng các hàm tạo này phải được sử dụng với từ khóa `new` để xây dựng các thể hiện của các đối tượng cơ bản. Nếu không, các hàm này thực sự thực hiện ép kiểu (xem Chương 4).

Các hàm tạo đối tượng cơ bản này tạo ra các kiểu giá trị đối tượng thay vì nguyên thủy:

```js
myName = "Kyle";
typeof myName;                      // "string"

myNickname = new String("getify");
typeof myNickname;                  // "object"
```

Nói cách khác, một thể hiện của một hàm tạo đối tượng cơ bản thực sự có thể được xem như một lớp vỏ bao quanh giá trị nguyên thủy tương ứng.

| CẢNH BÁO: |
| :--- |
| Nó gần như được coi là *thực hành tồi* (bad practice) khi khởi tạo trực tiếp các đối tượng cơ bản này. Các đối tác nguyên thủy thường dễ đoán hơn, hiệu quả hơn, và cung cấp *tự động đóng hộp* (auto-boxing) (xem phần "Các Đối Tượng Tự Động" bên dưới) bất cứ khi nào dạng đối tượng-bao-quanh cơ bản là cần thiết cho truy cập thuộc tính/phương thức. |

Các hàm `Symbol(..)` và `BigInt(..)` được đề cập trong đặc tả là "các hàm tạo" (constructors), mặc dù chúng không được sử dụng với từ khóa `new`, và các giá trị chúng tạo ra trong một chương trình JS thực sự là nguyên thủy.

Tuy nhiên, có các *đối tượng cơ bản* nội bộ cho hai kiểu này, được sử dụng cho sự ủy quyền prototype và *tự động đóng hộp* (auto-boxing).

Ngược lại, đối với các giá trị nguyên thủy `null` và `undefined`, không có các "hàm tạo" `Null()` hoặc `Undefined()`, cũng như không có các đối tượng cơ bản hoặc prototype tương ứng.

### Prototypes

Các thể hiện của các hàm tạo đối tượng cơ bản được liên kết `[[Prototype]]` với các đối tượng `prototype` của hàm tạo của chúng:

* `String.prototype`: định nghĩa thuộc tính `length`, cũng như các phương thức cụ thể của chuỗi, như `toUpperCase()`, v.v.

* `Number.prototype`: định nghĩa các phương thức cụ thể của số, như `toPrecision(..)`, `toFixed(..)`, v.v.

* `Boolean.prototype`: định nghĩa các phương thức mặc định `toString()` và `valueOf()`.

* `Symbol.prototype`: định nghĩa `description` (getter), cũng như các phương thức mặc định `toString()` và `valueOf()`.

* `BigInt.prototype`: định nghĩa các phương thức mặc định `toString()`, `toLocaleString()`, và `valueOf()`.

Bất kỳ thể hiện trực tiếp nào của các hàm tạo tích hợp đều có quyền truy cập `[[Prototype]]` được ủy quyền tới các thuộc tính/phương thức `prototype` tương ứng của nó. Hơn nữa, các giá trị nguyên thủy tương ứng cũng có quyền truy cập được ủy quyền như vậy, thông qua cách *tự động đóng hộp* (auto-boxing).

### Các Đối Tượng Tự Động (Automatic Objects)

Tôi đã đề cập đến *tự động đóng hộp* (auto-boxing) vài lần (bao gồm Chương 1 và 2, và một vài lần cho đến nay trong chương này). Cuối cùng đã đến lúc chúng ta giải thích khái niệm đó.

Việc truy cập một thuộc tính hoặc phương thức trên một giá trị yêu cầu giá trị đó phải là một đối tượng. Như chúng ta đã thấy trong Chương 1, các nguyên thủy *không phải* là đối tượng, vì vậy JS sau đó cần tạm thời chuyển đổi/đóng gói một nguyên thủy như vậy thành đối tác đối tượng cơ bản của nó[^AutoBoxing] để thực hiện quyền truy cập đó.

Ví dụ:

```js
myName = "Kyle";

myName.length;              // 4

myName.toUpperCase();       // "KYLE"
```

Truy cập thuộc tính `length` hoặc phương thức `toUpperCase()`, chỉ được phép trên một giá trị chuỗi nguyên thủy vì JS *tự động đóng hộp* (auto-boxes) nguyên thủy `string` thành một đối tượng cơ bản bao quanh, một thể hiện của `new String(..)`. Nếu không, tất cả các truy cập như vậy sẽ phải thất bại, vì các nguyên thủy không có bất kỳ thuộc tính nào.

Quan trọng hơn, khi giá trị nguyên thủy được *tự động đóng hộp* thành đối tác đối tượng cơ bản của nó, những đối tượng được tạo ra bên trong đó có quyền truy cập vào các thuộc tính/phương thức được xác định trước (như `length` và `toUpperCase()`) thông qua một liên kết `[[Prototype]]` tới prototype của đối tượng cơ bản tương ứng của chúng.

Vì vậy, một `string` được *tự động đóng hộp* là một thể hiện của `new String()`, và do đó được liên kết với `String.prototype`. Hơn nữa, điều tương tự cũng đúng với `number` (được đóng gói như một thể hiện của `new Number()`) và `boolean` (được đóng gói như một thể hiện của `new Boolean()`).

Mặc dù các "hàm tạo" `Symbol(..)` và `BigInt(..)` (được sử dụng không có `new`) tạo ra các giá trị nguyên thủy, các giá trị nguyên thủy này cũng có thể được *tự động đóng hộp* thành các dạng bao quanh đối tượng cơ bản nội bộ của chúng, cho mục đích truy cập được ủy quyền tới các thuộc tính/phương thức.

| LƯU Ý: |
| :--- |
| Xem cuốn sách "Đối Tượng & Lớp" (Objects & Classes) của loạt sách này để biết thêm về các liên kết `[[Prototype]]` và quyền truy cập được ủy quyền/kế thừa tới các đối tượng prototype của các hàm tạo đối tượng cơ bản. |

Vì `null` và `undefined` không có các đối tượng cơ bản tương ứng, không có *tự động đóng hộp* cho các giá trị này.

Một câu hỏi chủ quan để xem xét: *tự động đóng hộp* có phải là một hình thức ép kiểu không? Tôi nói là có, mặc dù một số người không đồng ý. Bên trong, một nguyên thủy được chuyển đổi thành một đối tượng, có nghĩa là một sự thay đổi trong kiểu giá trị đã xảy ra. Có, nó là tạm thời, nhưng rất nhiều phép ép kiểu là tạm thời. Hơn nữa, việc chuyển đổi khá là *ngầm định* (được ngụ ý bởi quyền truy cập thuộc tính/phương thức, nhưng chỉ xảy ra bên trong). Chúng ta sẽ xem xét lại bản chất của ép kiểu trong Chương 4.

## Các Đối Tượng Tích Hợp Khác (Other Built-in Objects)

Ngoài các hàm tạo đối tượng cơ bản, JS xác định một số hàm tạo tích hợp khác tạo ra các kiểu phụ đối tượng chuyên biệt hơn:

* `new Date(..)`
* `new Error(..)`
* `new Map(..)`, `new Set(..)`, `new WeakMap(..)`, `new WeakSet(..)` -- các bộ sưu tập có khóa (keyed collections)
* `new Int8Array(..)`, `new Uint32Array(..)`, v.v. -- các bộ sưu tập mảng định kiểu, được lập chỉ mục (indexed, typed-array collections)
* `new ArrayBuffer(..)`, `new SharedArrayBuffer(..)`, v.v. -- các bộ sưu tập dữ liệu cấu trúc (structured data collections)

## Mảng (Arrays)

Mảng là các đối tượng được chuyên biệt hóa để hành xử như các tập hợp các giá trị được lập chỉ mục bằng số, trái ngược với việc giữ các giá trị tại các thuộc tính được đặt tên như các đối tượng thuần làm.

Mảng có một dạng literal:

```js
favoriteNumbers = [ 3, 12, 42 ];

favoriteNumbers[2];                 // 42
```

Cùng một mảng cũng có thể được định nghĩa một cách mệnh lệnh bằng cách sử dụng hàm tạo `new Array()`:

```js
favoriteNumbers = new Array();
favoriteNumbers[0] = 3;
favoriteNumbers[1] = 12;
favoriteNumbers[2] = 42;
```

Mảng được liên kết `[[Prototype]]` với `Array.prototype`, cung cấp cho chúng quyền truy cập được ủy quyền tới một loạt các phương thức hướng mảng, chẳng hạn như `map(..)`, `includes(..)`, v.v.:

```js
favoriteNumbers.map(v => v * 2);
// [ 6, 24, 84 ]

favoriteNumbers.includes(42);       // true
```

Một số phương thức được định nghĩa trên `Array.prototype` -- ví dụ, `push(..)`, `pop(..)`, `sort(..)`, v.v. -- hoạt động bằng cách sửa đổi giá trị mảng tại chỗ (in place). Các phương thức khác -- ví dụ, `concat(..)`, `map(..)`, `slice(..)` -- hoạt động bằng cách tạo ra một mảng mới để trả về, giữ nguyên mảng ban đầu. Một danh mục thứ ba của các hàm mảng -- ví dụ, `indexOf(..)`, `includes(..)`, v.v. -- chỉ đơn thuần tính toán và trả về một kết quả (không phải mảng).

## Biểu Thức Chính Quy (Regular Expressions)

// TODO

## Hàm (Functions)

// TODO

## Đề xuất: Records/Tuples

Tại thời điểm viết bài này, một đề xuất (giai đoạn 2)[^RecordsTuplesProposal] tồn tại để thêm một tập hợp các tính năng mới vào JS, tương ứng chặt chẽ với các đối tượng thuần và mảng, nhưng với một số khác biệt đáng chú ý.

Records (Bản ghi) tương tự như các đối tượng thuần, nhưng là bất biến (được niêm phong, chỉ đọc), và (không giống như các đối tượng) được coi là giá trị nguyên thủy, cho các mục đích gán giá trị và so sánh bằng. Sự khác biệt về cú pháp là một dấu `#` trước dấu phân cách `{ }`. Records chỉ có thể chứa các giá trị nguyên thủy (bao gồm cả records và tuples).

Tuples (Bộ n-số) có mối quan hệ chính xác tương tự, nhưng với mảng, bao gồm cả dấu `#` trước các dấu phân cách `[ ]`.

Điều quan trọng cần lưu ý là trong khi những thứ này trông và có vẻ giống như đối tượng/mảng, chúng thực sự là các giá trị nguyên thủy (không phải đối tượng).

[^AutoBoxing]: "6.2.4.6 PutValue(V,W)", Step 5.a, ECMAScript 2022 Language Specification; <https://262.ecma-international.org/13.0/#sec-putvalue> ; Accessed August 2022

[^RecordsTuplesProposal]: "JavaScript Records & Tuples Proposal"; Robin Ricard, Rick Button, Nicolò Ribaudo;
<https://github.com/tc39/proposal-record-tuple> ; Accessed August 2022
