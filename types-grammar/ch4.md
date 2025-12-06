# You Don't Know JS Yet: Types & Grammar - 2nd Edition
# Chương 4: Ép kiểu giá trị (Coercing Values)

| NOTE: |
| :--- |
| Đang thực hiện |

Chúng ta đã bao quát kỹ lưỡng tất cả các *kiểu* giá trị khác nhau trong JS. Và trong suốt chặng đường, không ít lần, chúng ta đã đề cập đến khái niệm chuyển đổi -- thực ra là ép kiểu (coercing) -- từ một kiểu giá trị này sang kiểu giá trị khác.

Trong chương này, chúng ta sẽ đi sâu vào ép kiểu và khám phá tất cả những bí ẩn của nó.

## Ép kiểu: Tường minh (Explicit) so với Ngầm định (Implicit)

Một số nhà phát triển khẳng định rằng khi bạn chỉ định rõ ràng một sự thay đổi kiểu trong một thao tác, điều này không đủ điều kiện là một *ép kiểu* (coercion) mà chỉ là một ép kiểu (type-cast) hoặc chuyển đổi kiểu (type-conversion). Nói cách khác, tuyên bố là ép kiểu chỉ là ngầm định.

Tôi không đồng ý với mô tả này. Tôi sử dụng *ép kiểu* (coercion) để đặt nhãn cho bất kỳ chuyển đổi kiểu nào trong một ngôn ngữ kiểu động, cho dù nó rõ ràng trong code hay không. Đây là lý do: ranh giới giữa *tường minh* và *ngầm định* không rõ ràng và khách quan, nó khá chủ quan. Nếu bạn nghĩ một chuyển đổi kiểu là ngầm định (và do đó là *ép kiểu*), nhưng tôi nghĩ nó là tường minh (và do đó không phải là *ép kiểu*), sự phân biệt trở nên không liên quan.

Hãy ghi nhớ tính chủ quan đó khi chúng ta khám phá các dạng ép kiểu *tường minh* và *ngầm định* khác nhau. Trên thực tế, đây là một tiết lộ trước: hầu hết các ép kiểu có thể được lập luận là cả hai, vì vậy chúng ta sẽ xem xét chúng với quan điểm cân bằng như vậy.

### Ngầm định: Xấu hay ...?

Một ý kiến cực kỳ phổ biến trong giới phát triển JS là *ép kiểu là xấu*, cụ thể là *ép kiểu ngầm định là xấu*; sự gia tăng phổ biến của các công cụ nhận biết kiểu như TypeScript nói lên mạnh mẽ tình cảm này.

Nhưng cảm giác đó không phải là mới. Hơn 14 năm trước, cuốn sách "The Good Parts" của Douglas Crockford cũng đã công khai chỉ trích *ép kiểu ngầm định* là một trong những *phần tồi tệ* (bad parts). Ngay cả Brendan Eich, người tạo ra JS, thường xuyên tuyên bố rằng *ép kiểu ngầm định* là một sai lầm[^EichCoercion] trong thiết kế ban đầu của ngôn ngữ mà giờ ông hối tiếc.

Nếu bạn đã tham gia JS hơn vài tháng, bạn gần như chắc chắn đã nghe những ý kiến này được lên tiếng mạnh mẽ và chủ yếu. Và nếu bạn đã tham gia JS trong nhiều năm hoặc hơn, bạn có thể đã quyết định rồi.

Trên thực tế, tôi nghĩ bạn sẽ khó có thể kể tên bất kỳ nguồn dạy JS nổi tiếng nào khác tán thành mạnh mẽ việc ép kiểu (dưới hầu hết mọi hình thức của nó); tôi thì có -- và cuốn sách này chắc chắn là có! -- nhưng tôi cảm thấy chủ yếu giống như một giọng nói đơn độc hét lên vô ích trong hoang dã.

Tuy nhiên, đây là một quan sát tôi đã thực hiện qua nhiều năm: hầu hết những người công khai lên án *ép kiểu ngầm định*, thực sự sử dụng *ép kiểu ngầm định* trong code của riêng họ. Hmmmm...

Douglas Crockford nói hãy tránh sai lầm của *ép kiểu ngầm định*[^CrockfordCoercion], nhưng code của ông sử dụng các câu lệnh `if (..)` với các giá trị không phải boolean được đánh giá. [^CrockfordIfs] Nhiều người đã bác bỏ việc tôi chỉ ra điều đó trong quá khứ, với tuyên bố rằng chuyển đổi sang boolean không *thực sự* là ép kiểu. Ummm... ok?

Brendan Eich nói ông hối tiếc về *ép kiểu ngầm định*, nhưng ông lại công khai tán thành[^BrendanToString] các thành ngữ như `x + ""` (và những cái khác!) để ép kiểu giá trị trong `x` sang một chuỗi (chúng ta sẽ đề cập đến điều này sau); và đó chắc chắn là một *ép kiểu ngầm định*.

Vì vậy, chúng ta hiểu thế nào về sự mâu thuẫn này? Có phải nó chỉ đơn thuần là một mâu thuẫn nhỏ kiểu "làm theo lời tôi nói, không phải việc tôi làm" không? Hay còn điều gì khác nữa?

Tôi sẽ không đưa ra phán xét cuối cùng ở đây, nhưng tôi muốn bạn, người đọc, suy ngẫm sâu sắc câu hỏi đó, khi bạn tiếp tục trong suốt chương này và cuốn sách.

## Các thao tác trừu tượng (Abstracts)

Bây giờ tôi đã thách thức bạn kiểm tra ép kiểu sâu hơn mức bạn có thể đã từng thưởng thức trước đây, trước tiên hãy xem xét nền tảng của cách ép kiểu xảy ra, theo đặc tả JS.

Đặc tả chi tiết một số *thao tác trừu tượng*[^AbstractOperations] quy định chuyển đổi nội bộ từ một kiểu giá trị này sang kiểu giá trị khác. Điều quan trọng là phải nhận thức được các thao tác này, vì cơ chế ép kiểu trong ngôn ngữ trộn lẫn và kết hợp chúng theo nhiều cách khác nhau.

Các thao tác này *trông* như thể chúng là các hàm thực sự có thể được gọi, chẳng hạn như `ToString(..)` hoặc `ToNumber(..)`. Nhưng theo *trừu tượng*, chúng tôi muốn nói rằng chúng chỉ tồn tại về mặt khái niệm bằng những tên này; chúng không phải là các hàm chúng ta có thể gọi *trực tiếp* trong các chương trình của mình. Thay vào đó, chúng ta kích hoạt chúng một cách ngầm định/gián tiếp tùy thuộc vào các câu lệnh/biểu thức trong các chương trình của mình.

### ToBoolean

Việc ra quyết định (rẽ nhánh có điều kiện) luôn yêu cầu một giá trị boolean `true` hoặc `false`. Nhưng cực kỳ phổ biến khi muốn đưa ra các quyết định này dựa trên các điều kiện giá trị không phải boolean, chẳng hạn như liệu một chuỗi có rỗng hay có gì trong đó không.

Khi gặp các giá trị không phải boolean trong bối cảnh yêu cầu một boolean -- chẳng hạn như mệnh đề điều kiện của một câu lệnh `if` hoặc vòng lặp `for` -- thao tác trừu tượng `ToBoolean(..)`[^ToBoolean] được kích hoạt để tạo điều kiện thuận lợi cho việc ép kiểu.

Tất cả các giá trị trong JS đều nằm trong một trong hai nhóm: *truthy* hoặc *falsy*. Các giá trị truthy ép kiểu thông qua thao tác `ToBoolean()` thành `true`, trong khi các giá trị falsy ép kiểu thành `false`:

```
// ToBoolean() là trừu tượng

ToBoolean(undefined);               // false
ToBoolean(null);                    // false
ToBoolean("");                      // false
ToBoolean(0);                       // false
ToBoolean(-0);                      // false
ToBoolean(0n);                      // false
ToBoolean(NaN);                     // false
```

Quy tắc đơn giản: *bất kỳ giá trị nào khác* không có trong danh sách trên đều là truthy và ép kiểu thông qua `ToBoolean()` thành `true`:

```
ToBoolean("hello");                 // true
ToBoolean(42);                      // true
ToBoolean([ 1, 2, 3 ]);             // true
ToBoolean({ a: 1 });                // true
```

Ngay cả các giá trị như `"   "` (chuỗi chỉ có khoảng trắng), `[]` (mảng rỗng), và `{}` (đối tượng rỗng), thoạt nhìn có vẻ trực quan giống như chúng "false" hơn là "true", tuy nhiên vẫn ép kiểu thành `true`.

| WARNING: |
| :--- |
| *Có* những ngoại lệ hẹp, khó khăn đối với quy tắc truthy này. Ví dụ, nền tảng web đã loại bỏ tính năng mảng/bộ sưu tập `document.all` lâu đời, mặc dù nó không thể bị xóa hoàn toàn -- điều đó sẽ làm hỏng quá nhiều trang web. Ngay cả khi `document.all` vẫn được định nghĩa, nó hoạt động như một "đối tượng falsy"[^ExoticFalsyObjects] -- `undefined` sau đó ép kiểu thành `false`; điều này có nghĩa là các kiểm tra điều kiện cũ như `if (document.all) { .. }` không còn vượt qua nữa. |

Thao tác ép kiểu `ToBoolean()` về cơ bản là một bảng tra cứu thay vì một thuật toán các bước để sử dụng trong việc ép kiểu một giá trị không phải boolean thành một boolean. Do đó, một số nhà phát triển khẳng định rằng đây không *thực sự* là ép kiểu theo cách các thao tác ép kiểu trừu tượng khác làm. Tôi nghĩ điều đó là sai lầm. `ToBoolean()` chuyển đổi từ các kiểu giá trị không phải boolean thành một boolean, và đó là ép kiểu rõ ràng (ngay cả khi nó là một bảng tra cứu rất đơn giản thay vì một thuật toán).

Hãy nhớ rằng: những quy tắc ép kiểu boolean này chỉ áp dụng khi `ToBoolean()` thực sự được kích hoạt. Có những cấu trúc/thành ngữ trong ngôn ngữ JS có thể có vẻ liên quan đến ép kiểu boolean nhưng thực ra không làm như vậy. Thêm về những điều này sau.

### ToPrimitive

Bất kỳ giá trị nào chưa phải là nguyên thủy đều có thể được giảm xuống thành nguyên thủy bằng cách sử dụng thao tác trừu tượng `ToPrimitive()` (cụ thể là `OrdinaryToPrimitive()`[^OrdinaryToPrimitive]). Nói chung, `ToPrimitive()` được đưa ra một *gợi ý* để cho nó biết liệu `number` hay `string` được ưu tiên hơn.

```
// ToPrimitive() là trừu tượng

ToPrimitive({ a: 1 },"string");          // "[object Object]"

ToPrimitive({ a: 1 },"number");          // NaN
```

Thao tác `ToPrimitive()` sẽ tìm kiếm trên đối tượng được cung cấp, cho một phương thức `toString()` hoặc một phương thức `valueOf()`; thứ tự nó tìm kiếm chúng được kiểm soát bởi *gợi ý*. `"string"` có nghĩa là kiểm tra theo thứ tự `toString()` / `valueOf()`, trong khi `"number"` (hoặc không có *gợi ý*) có nghĩa là kiểm tra theo thứ tự `valueOf()` / `toString()`.

Nếu phương thức trả về một giá trị khớp với kiểu *được gợi ý*, thao tác kết thúc. Nhưng nếu phương thức không trả về một giá trị của kiểu *được gợi ý*, `ToPrimitive()` sau đó sẽ tìm kiếm và gọi phương thức khác (nếu tìm thấy).

Nếu các nỗ lực gọi phương thức không tạo ra được một giá trị của kiểu *được gợi ý*, giá trị trả về cuối cùng bị ép buộc ép kiểu thông qua thao tác trừu tượng tương ứng: `ToString()` hoặc `ToNumber()`.

### ToString

Hầu như bất kỳ giá trị nào chưa phải là một chuỗi đều có thể được ép kiểu thành biểu diễn chuỗi, thông qua `ToString()`. [^ToString] Điều này thường khá trực quan, đặc biệt với các giá trị nguyên thủy:

```
// ToString() là trừu tượng

ToString(42.0);                 // "42"
ToString(-3);                   // "-3"
ToString(Infinity);             // "Infinity"
ToString(NaN);                  // "NaN"
ToString(42n);                  // "42"

ToString(true);                 // "true"
ToString(false);                // "false"

ToString(null);                 // "null"
ToString(undefined);            // "undefined"
```

Có *một số* kết quả có thể khác với trực giác thông thường. Như đã đề cập trong Chương 2, các số rất lớn hoặc rất nhỏ sẽ được biểu diễn bằng ký hiệu khoa học:

```
ToString(Number.MAX_VALUE);     // "1.7976931348623157e+308"
ToString(Math.EPSILON);         // "2.220446049250313e-16"
```

Một kết quả phản trực giác khác đến từ `-0`:

```
ToString(-0);                   // "0" -- cái quái gì vậy?
```

Đây không phải là lỗi, nó chỉ là một hành vi có chủ ý từ những ngày đầu của JS, dựa trên giả định rằng các nhà phát triển thường sẽ không bao giờ muốn thấy đầu ra số không âm.

Một kiểu giá trị nguyên thủy *không được phép* ép kiểu (ngầm định, ít nhất) thành chuỗi là `symbol`:

```
ToString(Symbol("ok"));         // Ngoại lệ TypeError được ném ra
```

| WARNING: |
| :--- |
| Gọi hàm cụ thể `String()`[^StringFunction] (không có toán tử `new`) thường được coi là *chỉ đơn thuần* gọi thao tác trừu tượng `ToString()` để ép kiểu một giá trị thành một chuỗi. Mặc dù điều đó hầu hết là đúng, nhưng không hoàn toàn như vậy. `String(Symbol("ok"))` hoạt động, trong khi bản thân `ToString(Symbol(..))` trừu tượng ném ra một ngoại lệ. Thêm về `String(..)` sau trong chương này. |

#### `toString()` Mặc định (Default `toString()`)

Khi `ToString()` được kích hoạt với một kiểu giá trị đối tượng, nó ủy quyền cho thao tác `ToPrimitive()` (như đã giải thích trước đó), với `"string"` làm kiểu *được gợi ý*:

```
ToString(new String("abc"));        // "abc"
ToString(new Number(42));           // "42"

ToString({ a: 1 });                 // "[object Object]"
ToString([ 1, 2, 3 ]);              // "1,2,3"
```

Nhờ sự ủy quyền `ToPrimitive(..,"string")`, tất cả các đối tượng này đều có phương thức `toString()` mặc định của chúng (được kế thừa qua `[[Prototype]]`) được gọi.

### ToNumber

Các giá trị không phải số *trông giống* số, chẳng hạn như chuỗi số, thường có thể được ép kiểu thành biểu diễn số, sử dụng `ToNumber()`: [^ToNumber]

```
// ToNumber() là trừu tượng

ToNumber("42");                     // 42
ToNumber("-3");                     // -3
ToNumber("1.2300");                 // 1.23
ToNumber("   8.0    ");             // 8
```

Nếu toàn bộ giá trị không *hoàn toàn* (ngoài khoảng trắng) giống một số hợp lệ, kết quả sẽ là `NaN`:

```
ToNumber("123px");                  // NaN
ToNumber("hello");                  // NaN
```

Các giá trị nguyên thủy khác có các tương đương số được chỉ định nhất định:

```
ToNumber(true);                     // 1
ToNumber(false);                    // 0

ToNumber(null);                     // 0
ToNumber(undefined);                // NaN
```

Có một số chỉ định khá đáng ngạc nhiên cho `ToNumber()`:

```
ToNumber("");                       // 0
ToNumber("       ");                // 0
```

| NOTE: |
| :--- |
| Tôi gọi những điều này là "đáng ngạc nhiên" vì tôi nghĩ sẽ hợp lý hơn nhiều nếu chúng ép kiểu thành `NaN`, giống như cách `undefined` làm. |

Một số giá trị nguyên thủy *không được phép* ép kiểu thành số, và dẫn đến các ngoại lệ thay vì `NaN`:

```
ToNumber(42n);                      // Ngoại lệ TypeError được ném ra
ToNumber(Symbol("42"));             // Ngoại lệ TypeError được ném ra
```

| WARNING: |
| :--- |
| Gọi hàm cụ thể `Number()`[^NumberFunction] (không có toán tử `new`) thường được coi là *chỉ đơn thuần* gọi thao tác trừu tượng `ToNumber()` để ép kiểu một giá trị thành một số. Mặc dù điều đó hầu hết là đúng, nhưng không hoàn toàn như vậy. `Number(42n)` hoạt động, trong khi bản thân `ToNumber(42n)` trừu tượng ném ra một ngoại lệ. |

#### Các chuyển đổi số trừu tượng khác (Other Abstract Numeric Conversions)

Ngoài `ToNumber()`, đặc tả định nghĩa `ToNumeric()`, kích hoạt `ToPrimitive()` trên một giá trị, sau đó ủy quyền có điều kiện cho `ToNumber()` nếu giá trị *chưa* phải là kiểu giá trị `bigint`.

Cũng có rất nhiều thao tác trừu tượng liên quan đến việc chuyển đổi giá trị thành các tập hợp con rất cụ thể của kiểu `number` chung:

* `ToIntegerOrInfinity()`
* `ToInt32()`
* `ToUint32()`
* `ToInt16()`
* `ToUint16()`
* `ToInt8()`
* `ToUint8()`
* `ToUint8Clamp()`

Các thao tác khác liên quan đến `bigint`:

* `ToBigInt()`
* `StringToBigInt()`
* `ToBigInt64()`
* `ToBigUint64()`

Bạn có thể suy luận mục đích của các thao tác này từ tên của chúng, và/hoặc từ việc tham khảo các thuật toán của chúng trong đặc tả. Đối với hầu hết các thao tác JS, nhiều khả năng một thao tác cấp cao hơn như `ToNumber()` được kích hoạt, thay vì các thao tác cụ thể này.

#### `valueOf()` Mặc định (Default `valueOf()`)

Khi `ToNumber()` được kích hoạt trên một kiểu giá trị đối tượng, thay vào đó nó ủy quyền cho thao tác `ToPrimitive()` (như đã giải thích trước đó), với `"number"` làm kiểu *được gợi ý*:

```
ToNumber(new String("abc"));        // NaN
ToNumber(new Number(42));           // 42

ToNumber({ a: 1 });                 // NaN
ToNumber([ 1, 2, 3 ]);              // NaN
ToNumber([]);                       // 0
```

Nhờ sự ủy quyền `ToPrimitive(..,"number")`, tất cả các đối tượng này đều có phương thức `valueOf()` mặc định của chúng (được kế thừa qua `[[Prototype]]`) được gọi.

### So sánh bằng (Equality Comparison)

Khi JS cần xác định xem hai giá trị có phải là *cùng một giá trị* hay không, nó kích hoạt thao tác `SameValue()`[^SameValue], thao tác này ủy quyền cho nhiều thao tác phụ liên quan.

Thao tác này rất hẹp và nghiêm ngặt, và không thực hiện ép kiểu hoặc bất kỳ ngoại lệ trường hợp đặc biệt nào khác. Nếu hai giá trị *chính xác* giống nhau, kết quả là `true`, ngược lại là `false`:

```
// SameValue() là trừu tượng

SameValue("hello","\x68ello");          // true
SameValue("\u{1F4F1}","\uD83D\uDCF1");  // true
SameValue(42,42);                       // true
SameValue(NaN,NaN);                     // true

SameValue("\u00e9","\u0065\u0301");     // false
SameValue(0,-0);                        // false
SameValue([1,2,3],[1,2,3]);             // false
```

Một biến thể của các thao tác này là `SameValueZero()` và các thao tác phụ liên quan của nó. Sự khác biệt chính là các thao tác này coi `0` và `-0` là không thể phân biệt được.

```
// SameValueZero() là trừu tượng

SameValueZero(0,-0);                    // true
```

Nếu các giá trị là số (`number` hoặc `bigint`), `SameValue()` và `SameValueZero()` đều ủy quyền cho các thao tác phụ có cùng tên, chuyên biệt cho từng kiểu `number` và `bigint`, tương ứng.

Nếu không, `SameValueNonNumeric()` là thao tác phụ được ủy quyền nếu các giá trị được so sánh đều không phải là số:

```
// SameValueNonNumeric() là trừu tượng

SameValueNonNumeric("hello","hello");   // true

SameValueNonNumeric([1,2,3],[1,2,3]);   // false
```

#### So sánh bằng trừu tượng cấp cao hơn (Higher-Abstracted Equality)

Khác với `SameValue()` và các biến thể của nó, đặc tả cũng định nghĩa hai thao tác so sánh bằng trừu tượng cấp cao hơn quan trọng:

* `IsStrictlyEqual()`[^StrictEquality]
* `IsLooselyEqual()`[^LooseEquality]

Thao tác `IsStrictlyEqual()` ngay lập tức trả về `false` nếu các kiểu giá trị được so sánh khác nhau.

Nếu các kiểu giá trị giống nhau, `IsStrictlyEqual()` ủy quyền cho các thao tác phụ để so sánh các giá trị `number` hoặc `bigint`. [^NumericAbstractOps] Bạn có thể mong đợi một cách logic rằng các thao tác phụ được ủy quyền này là các thao tác `SameValue()` / `SameValueZero()` chuyên biệt cho số đã nói ở trên. Tuy nhiên, `IsStrictlyEqual()` thay vào đó ủy quyền cho `Number:equal()`[^NumberEqual] hoặc `BigInt:equal()`[^BigIntEqual].

Sự khác biệt giữa `Number:SameValue()` và `Number:equal()` là cái sau định nghĩa các trường hợp góc cho so sánh `0` vs `-0`:

```
// tất cả những cái này là thao tác trừu tượng

Number:SameValue(0,-0);             // false
Number:SameValueZero(0,-0);         // true
Number:equal(0,-0);                 // true
```

Các thao tác này cũng khác nhau trong so sánh `NaN` vs `NaN`:

```
Number:SameValue(NaN,NaN);          // true
Number:equal(NaN,NaN);              // false
```

| WARNING: |
| :--- |
| Vì vậy, nói cách khác, bất chấp tên gọi của nó, `IsStrictlyEqual()` không hoàn toàn "nghiêm ngặt" (strict) như `SameValue()`, ở chỗ nó *nói dối* khi liên quan đến các so sánh của `-0` hoặc `NaN`. |

Thao tác `IsLooselyEqual()` cũng kiểm tra các kiểu giá trị được so sánh; nếu chúng giống nhau, nó ngay lập tức ủy quyền cho `IsStrictlyEqual()`.

Nhưng nếu các kiểu giá trị được so sánh khác nhau, `IsLooselyEqual()` thực hiện nhiều bước *so sánh bằng ép kiểu* (coercive equality). Điều quan trọng cần lưu ý là thuật toán này luôn cố gắng giảm phép so sánh xuống nơi cả hai kiểu giá trị giống nhau (và nó có xu hướng ưu tiên `number` / `bigint`).

Các bước của phần *so sánh bằng ép kiểu* của thuật toán có thể được tóm tắt đại khái như sau:

1. Nếu một trong hai giá trị là `null` và giá trị kia là `undefined`, `IsLooselyEqual()` trả về `true`. Nói cách khác, thuật toán này áp dụng so sánh bằng *nullish*, ở chỗ `null` và `undefined` tương đương về mặt ép kiểu với nhau (và không với giá trị nào khác).

2. Nếu một trong hai giá trị là `number` và giá trị kia là `string`, giá trị `string` được ép kiểu thành `number` thông qua `ToNumber()`.

3. Nếu một trong hai giá trị là `bigint` và giá trị kia là `string`, giá trị `string` được ép kiểu thành `bigint` thông qua `StringToBigInt()`.

4. Nếu một trong hai giá trị là `boolean`, nó được ép kiểu thành `number`.

5. Nếu một trong hai giá trị không phải là nguyên thủy (đối tượng, v.v.), nó được ép kiểu thành một nguyên thủy với `ToPrimitive()`; mặc dù một *gợi ý* không được cung cấp một cách rõ ràng, hành vi mặc định sẽ giống như thể `"number"` là gợi ý.

Mỗi khi một ép kiểu được thực hiện trong các bước trên, thuật toán được kích hoạt lại *đệ quy* với (các) giá trị mới. Quá trình đó tiếp tục cho đến khi các kiểu giống nhau, và sau đó việc so sánh được ủy quyền cho thao tác `IsStrictlyEqual()`.

Chúng ta có thể rút ra điều gì từ thuật toán này? Đầu tiên, chúng ta thấy có một sự thiên vị đối với so sánh `number` (hoặc `bigint`); nó không bao giờ ép kiểu các giá trị thành các kiểu giá trị `string` hoặc `boolean`.

Quan trọng là, chúng ta thấy rằng cả `IsLooselyEqual()` và `IsStrictlyEqual()` đều nhạy cảm với kiểu. `IsStrictlyEqual()` ngay lập tức thoát nếu các kiểu không khớp, trong khi `IsLooselyEqual()` thực hiện thêm công việc để ép kiểu các kiểu giá trị không khớp thành cùng kiểu giá trị (một lần nữa, lý tưởng nhất là `number` hoặc `bigint`).

Hơn nữa, nếu/khi các kiểu giống nhau, cả hai thao tác đều giống hệt nhau -- `IsLooselyEqual()` ủy quyền cho `IsStrictlyEqual()`.

### So sánh quan hệ (Relational Comparison)

Khi các giá trị được so sánh theo quan hệ -- nghĩa là, một giá trị có "nhỏ hơn" giá trị kia không? -- có một thao tác trừu tượng cụ thể được kích hoạt: `IsLessThan()`. [^LessThan]

```
// IsLessThan() là trừu tượng

IsLessThan(1,2, /*LeftFirst=*/ true );            // true
```

Không có thao tác `IsGreaterThan()`; thay vào đó, hai đối số đầu tiên của `IsLessThan()` có thể được đảo ngược để thực hiện so sánh "lớn hơn". Để bảo tồn ngữ nghĩa đánh giá từ trái sang phải (trong trường hợp các tác dụng phụ sắc thái), `isLessThan()` cũng nhận một đối số thứ ba (`LeftFirst`); nếu `false`, điều này cho biết một so sánh đã bị đảo ngược và tham số thứ hai nên được đánh giá trước tham số thứ nhất.

```
IsLessThan(1,2, /*LeftFirst=*/ true );            // true

// tương đương với một "IsGreaterThan()" hư cấu
IsLessThan(2,1, /*LeftFirst=*/ false );          // false
```

Tương tự như `IsLooselyEqual()`, thao tác `IsLessThan()` có tính *ép kiểu*, nghĩa là trước tiên nó đảm bảo rằng các kiểu giá trị của hai giá trị của nó khớp nhau, và ưu tiên các so sánh số. Không có `IsStrictLessThan()` cho so sánh quan hệ không ép kiểu.

Là một ví dụ về so sánh quan hệ ép kiểu, nếu kiểu của một giá trị là `string` và kiểu của giá trị kia là `bigint`, `string` được ép kiểu thành `bigint` với thao tác `StringToBigInt()` đã nói ở trên. Khi các kiểu giống nhau, `IsLessThan()` tiến hành như được mô tả trong các phần sau.

#### So sánh chuỗi (String Comparison)

Khi cả hai giá trị đều là kiểu `string`, `IsLessThan()` kiểm tra xem giá trị bên trái có phải là tiền tố (prefix) (các ký tự *n* đầu tiên[^StringPrefix]) của bên phải hay không; nếu vậy, `true` được trả về.

Nếu không chuỗi nào là tiền tố của chuỗi kia, vị trí ký tự đầu tiên (hướng bắt đầu đến kết thúc, không phải trái sang phải) khác nhau giữa hai chuỗi, được so sánh về các giá trị đơn vị mã (số) tương ứng của chúng; kết quả sau đó được trả về.

Nói chung, các đơn vị mã tuân theo thứ tự từ điển (lexicographic) trực quan (còn gọi là dictionary):

```
IsLessThan("a","b", /*LeftFirst=*/ true );        // true
```

Ngay cả các chữ số cũng được coi là ký tự (không phải số):

```
IsLessThan("101","12", /*LeftFirst=*/ true );     // true
```

Thậm chí còn có một chút *hài hước* nhúng trong thứ tự đơn vị mã unicode:

```
IsLessThan("🐔","🥚", /*LeftFirst=*/ true );      // true
```

Ít nhất bây giờ chúng ta đã trả lời câu hỏi muôn thuở là *cái nào đến trước*?!

#### So sánh số (Numeric Comparison)

Đối với các so sánh số, `IsLessThan()` ủy quyền cho thao tác `Number:lessThan()` hoặc `BigInt:lessThan()`[^NumericAbstractOps], tương ứng:

```
IsLessThan(41,42, /*LeftFirst=*/ true );         // true

IsLessThan(-0,0, /*LeftFirst=*/ true );          // false

IsLessThan(NaN,1 /*LeftFirst=*/ true );          // false

IsLessThan(41n,42n, /*LeftFirst=*/ true );       // true
```

## Các ép kiểu cụ thể (Concrete Coercions)

Bây giờ chúng ta đã bao gồm tất cả các thao tác trừu tượng mà JS định nghĩa để xử lý các ép kiểu khác nhau, đã đến lúc chuyển sự chú ý của chúng ta sang các câu lệnh/biểu thức cụ thể mà chúng ta có thể sử dụng trong các chương trình của mình để kích hoạt các thao tác này.

### Chuyển sang Boolean (To Boolean)

Để ép kiểu một giá trị không phải kiểu `boolean` thành kiểu đó, chúng ta cần thao tác trừu tượng `ToBoolean()`, như đã mô tả trước đó trong chương này.

Trước khi chúng ta khám phá *cách* kích hoạt nó, hãy thảo luận *tại sao* bạn lại muốn buộc một ép kiểu `ToBoolean()` xảy ra.

Từ góc độ khả năng đọc code, việc *tường minh* về ép kiểu có thể thích hợp hơn (mặc dù không phải phổ biến). Nhưng về mặt chức năng, lý do phổ biến nhất để buộc một `boolean` là khi bạn chuyển dữ liệu đến một nguồn bên ngoài -- ví dụ: gửi dữ liệu dưới dạng JSON đến điểm cuối API -- và vị trí đó mong đợi `true` / `false` mà không cần thực hiện ép kiểu.

Có một số cách mà `ToBoolean()` có thể được kích hoạt. Có lẽ cách *tường minh* (rõ ràng) nhất là hàm `Boolean(..)`:

```js
Boolean("hello");               // true
Boolean(42);                    // true

Boolean("");                    // false
Boolean(0);                     // false
```

Như đã đề cập trong Chương 3, hãy nhớ rằng `Boolean(..)` đang được gọi mà không có từ khóa `new`, để kích hoạt thao tác trừu tượng `ToBoolean()`.

Không quá phổ biến khi thấy các nhà phát triển JS sử dụng hàm `Boolean(..)` cho các ép kiểu tường minh như vậy. Thường xuyên hơn, các nhà phát triển sẽ sử dụng thành ngữ `!` kép:

```js
!!"hello";                      // true
!!42;                           // true

!!"";                           // false
!!0;                            // false
```

`!!` không phải là toán tử riêng của nó, ngay cả khi nó có vẻ như vậy. Nó thực sự là hai cách sử dụng của toán tử một ngôi `!`. Toán tử này đầu tiên ép buộc bất kỳ cái gì không phải `boolean`, sau đó phủ định nó. Để hoàn tác sự phủ định, phép `!` thứ hai lật nó lại.

Vậy... cách nào trong hai cách, `Boolean(..)` hay `!!`, bạn coi là ép kiểu tường minh hơn?

Với sự đảo ngược mà `!` thực hiện, sau đó phải được hoàn tác bằng một `!` khác, tôi nói rằng `Boolean(..)` là *tường minh* hơn -- ở công việc ép kiểu một cái gì không phải `boolean` thành một `boolean` -- so với `!!`. Nhưng khảo sát code JS mã nguồn mở, `!!` được sử dụng thường xuyên hơn nhiều.

Nếu chúng ta định nghĩa *tường minh* là, "thực hiện hành động một cách trực tiếp và rõ ràng nhất", `Boolean(..)` vượt qua `!!`. Nhưng nếu chúng ta định nghĩa *tường minh* là, "thực hiện hành động dễ nhận biết nhất", `!!` có thể có lợi thế. Có câu trả lời dứt khoát nào ở đây không?

Trong khi bạn đang suy ngẫm câu hỏi đó, hãy xem xét một cơ chế JS khác kích hoạt `ToBoolean()` bên dưới lớp vỏ:

```js
specialNumber = 42;

if (specialNumber) {
    // ..
}
```

Câu lệnh `if` yêu cầu một `boolean` cho điều kiện để đưa ra quyết định luồng điều khiển của nó. Nếu bạn chuyển cho nó một cái gì không phải `boolean`, một *ép kiểu* `ToBoolean()` được thực hiện.

Không giống như các biểu thức ép kiểu `ToBoolean()` trước đó, như `Boolean(..)` hoặc `!!`, ép kiểu `if` này là tạm thời, ở chỗ chương trình JS của chúng ta không bao giờ nhìn thấy kết quả của ép kiểu; nó chỉ được sử dụng nội bộ bởi `if`. Một số người có thể cảm thấy nó không *thực sự* là ép kiểu nếu chương trình không bảo tồn/sử dụng giá trị. Nhưng tôi hoàn toàn không đồng ý, bởi vì ép kiểu chắc chắn ảnh hưởng đến hành vi của chương trình.

Nhiều loại câu lệnh khác cũng kích hoạt ép kiểu `ToBoolean()`, bao gồm điều kiện ba ngôi `? :`, và các vòng lặp `for` / `while`. Chúng ta cũng có các toán tử `&&` (VÀ logic) và `||` (HOẶC logic). Ví dụ:

```js
isLoggedIn = user.sessionID || req.cookie["Session-ID"];

isAdmin = isLoggedIn && ("admin" in user.permissions);
```

Đối với cả hai toán tử, biểu thức bên trái được đánh giá đầu tiên; nếu nó chưa phải là một `boolean`, một ép kiểu `ToBoolean()` được kích hoạt để tạo ra một giá trị cho quyết định điều kiện.

| NOTE: |
| :--- |
| Để giải thích ngắn gọn các toán tử này: đối với `||`, nếu giá trị biểu thức bên trái (sau khi ép kiểu, nếu cần thiết) là `true`, giá trị trước khi ép kiểu được trả về; nếu không, biểu thức bên phải được đánh giá và trả về (không ép kiểu). Đối với `&&`, nếu giá trị biểu thức bên trái (sau khi ép kiểu, nếu cần thiết) là `false`, giá trị trước khi ép kiểu được trả về; nếu không, biểu thức bên phải được đánh giá và trả về (không ép kiểu). Nói cách khác, cả `&&` và `||` đều buộc ép kiểu `ToBoolean()` của toán hạng bên trái để đưa ra quyết định, nhưng kết quả cuối cùng của không toán tử nào thực sự được ép kiểu thành `boolean`. |

Trong đoạn code trước, bất chấp ý nghĩa đặt tên, không chắc rằng `isLoggedIn` thực sự sẽ là một `boolean`; và nếu nó là truthy, `isAdmin` cũng sẽ không phải là một `boolean`. Loại code đó khá phổ biến, nhưng chắc chắn là nguy hiểm nếu các kiểu `boolean` kết quả được giả định thực sự không có ở đó. Chúng ta sẽ xem xét lại ví dụ này, và các toán tử này, trong chương tiếp theo.

Những loại câu lệnh/biểu thức này (ví dụ: `if (..)`, `||`, `&&`, v.v.) có minh họa ép kiểu *tường minh* hay ép kiểu *ngầm định* trong việc đưa ra quyết định điều kiện của chúng không?

Một lần nữa, tôi nghĩ nó phụ thuộc vào quan điểm của bạn. Đặc tả quy định khá rõ ràng rằng chúng chỉ đưa ra quyết định của mình với các giá trị điều kiện `boolean`, yêu cầu ép kiểu nếu nhận được một cái gì không phải `boolean`. Mặt khác, một lập luận mạnh mẽ cũng có thể được đưa ra rằng bất kỳ ép kiểu nội bộ nào cũng là hiệu ứng (ngầm định) thứ cấp đối với công việc chính của `if` / `&&`/ v.v.

Hơn nữa, như đã đề cập trước đó trong cuộc thảo luận `ToBoolean()`, một số người không coi *bất kỳ* kích hoạt nào của `ToBoolean()` là một ép kiểu.

Tuy nhiên, tôi nghĩ điều đó là hơi quá. Quan điểm của tôi: `Boolean(..)` là dạng ép kiểu *tường minh* thích hợp nhất. Tôi nghĩ `!!`, `if`, `for`, `while`, `&&`, và `||` đều đang ép kiểu *ngầm định* các giá trị không phải `boolean`, nhưng tôi ổn với điều đó.

Vì hầu hết các nhà phát triển, bao gồm những cái tên nổi tiếng như Doug Crockford, trong thực tế cũng sử dụng các ép kiểu ngầm định (`boolean`) trong code của họ[^CrockfordIfs], tôi nghĩ chúng ta có thể nói rằng ít nhất *một số dạng* ép kiểu *ngầm định* được chấp nhận rộng rãi, bất chấp những luận điệu phổ biến ngược lại.

### Chuyển sang chuỗi (To String)

Cũng như với `ToBoolean()`, có một số cách để kích hoạt ép kiểu `ToString()` (như đã thảo luận trước đó trong chương này). Quyết định về cách tiếp cận nào là tương tự chủ quan.

Giống như hàm `Boolean(..)` , hàm `String(..)` (không có từ khóa `new`) là một cách chính để kích hoạt ép kiểu `ToString()` *tường minh*:

```js
String(true);                   // "true"
String(42);                     // "42"
String(-0);                     // "0"
String(Infinity);               // "Infinity"

String(null);                   // "null"
String(undefined);              // "undefined"
```

Tuy nhiên, `String(..)` không *chỉ* là việc kích hoạt `ToString()`. Ví dụ:

```js
String(Symbol("ok"));           // "Symbol(ok)"
```

Điều này hoạt động, bởi vì ép kiểu *tường minh* của các giá trị `symbol` được cho phép. Nhưng trong trường hợp một symbol bị ép kiểu *ngầm định* thành một chuỗi (ví dụ: `Symbol("ok") + ""`), thao tác `ToString()` bên dưới ném ra một ngoại lệ. Điều đó chứng minh rằng `String(..)` không chỉ là một kích hoạt của `ToString()`. Thêm về ép kiểu chuỗi *ngầm định* của các symbol trong chốc lát.

Nếu bạn gọi `String(..)` với một giá trị đối tượng (ví dụ: mảng, v.v.), nó sẽ kích hoạt thao tác `ToPrimitive()` (thông qua thao tác `ToString()`), thao tác này sau đó tìm kiếm và gọi phương thức `toString()` của giá trị đó:

```js
String([1,2,3]);                // "1,2,3"

String(x => x + 1);             // "x => x + 1"
```

Ngoài `String(..)`, bất kỳ giá trị nguyên thủy, không nullish nào (không phải `null` cũng không phải `undefined`) đều có thể được tự động đóng hộp (auto-boxed) (xem Chương 3) trong wrapper đối tượng tương ứng của nó, cung cấp một phương thức `toString()` có thể gọi được.

```js
true.toString();                // "true"
42..toString();                 // "42"
-0..toString();                 // "0"
Infinity.toString();            // "Infinity"
Symbol("ok").toString();        // "Symbol(ok)"
```

| NOTE: |
| :--- |
| Hãy nhớ rằng, các phương thức `toString()` này *không* nhất thiết kích hoạt thao tác `ToString()`, chúng chỉ định nghĩa các quy tắc của riêng chúng về cách biểu diễn giá trị dưới dạng một chuỗi. |

Như được hiển thị với `String(..)` ngay vừa rồi, các loại đối tượng phụ khác nhau -- chẳng hạn như mảng, hàm, biểu thức chính quy, các instance `Date` và `Error`, v.v. -- tất cả đều định nghĩa các phương thức `toString()` cụ thể của riêng chúng, có thể được gọi trực tiếp:

```js
[1,2,3].toString();             // "1,2,3"

(x => x + 1).toString();        // "x => x + 1"
```

Hơn nữa, bất kỳ đối tượng thuần túy nào (theo mặc định) được liên kết `[[Prototype]]` với `Object.prototype` đều có sẵn một phương thức `toString()` mặc định:

```js
({ a : 1 }).toString();         // "[object Object]"
```

Cách tiếp cận `toString()` để ép kiểu là *tường minh* hay *ngầm định*? Một lần nữa, nó phụ thuộc. Nó chắc chắn là một cơ chế tự mô tả, nghiêng về *tường minh*. Nhưng nó thường dựa vào tự động đóng hộp (auto-boxing), bản thân nó là một ép kiểu khá *ngầm định*.

Hãy xem xét một thành ngữ phổ biến khác -- và được tán thành nổi tiếng! -- để ép kiểu một giá trị thành một chuỗi. Nhớ lại từ "Nối chuỗi" trong Chương 2, toán tử `+` được nạp chồng để ưu tiên nối chuỗi nếu một trong hai toán hạng đã là một chuỗi, và do đó ép kiểu toán hạng không phải chuỗi thành một chuỗi nếu cần thiết.

Hãy xem xét:

```js
true + "";                      // "true"
42 + "";                        // "42"
null + "";                      // "null"
undefined + "";                 // "undefined"
```

Thành ngữ `+ ""` cho ép kiểu chuỗi tận dụng sự nạp chồng của `+`, mà không làm thay đổi giá trị chuỗi được ép kiểu cuối cùng. Nhân tiện, tất cả những cái này hoạt động giống nhau với các toán hạng bị đảo ngược (tức là, `"" + ..`).

| WARNING: |
| :--- |
| Một quan niệm sai lầm cực kỳ phổ biến là `String(x)` và `x + ""` về cơ bản là các ép kiểu tương đương, tương ứng chỉ là *tường minh* so với *ngầm định* về hình thức. Nhưng, điều đó không hoàn toàn đúng! Chúng ta sẽ xem xét lại điều này trong phần "Chuyển đổi sang nguyên thủy" sau đó trong chương này. |

Một số người cảm thấy đây là một ép kiểu *tường minh*, nhưng tôi nghĩ nó rõ ràng là *ngầm định* hơn, ở chỗ nó đang tận dụng sự nạp chồng của `+`; hơn nữa, `""` được sử dụng gián tiếp để kích hoạt ép kiểu mà không sửa đổi nó. Hơn nữa, hãy xem xét điều gì sẽ xảy ra khi thành ngữ này được áp dụng với một giá trị symbol:

```js
Symbol("ok") + "";              // Ngoại lệ TypeError được ném ra
```

| WARNING: |
| :--- |
| Cho phép ép kiểu *tường minh* của các symbol (`String(Symbol("ok"))`, nhưng không cho phép ép kiểu *ngầm định* (`Symbol("ok") + ""`), là khá cố ý bởi TC39. [^SymbolString] Người ta cảm thấy rằng các symbol, là các nguyên thủy thường được sử dụng ở những nơi mà chuỗi được sử dụng, có thể quá dễ bị nhầm lẫn là chuỗi. Do đó, họ muốn đảm bảo các nhà phát triển thể hiện ý định ép kiểu một symbol thành một chuỗi, hy vọng tránh được nhiều sự nhầm lẫn được dự đoán trước đó. Đây là một trong những trường hợp *cực kỳ hiếm* mà thiết kế ngôn ngữ khẳng định một ý kiến về, và thực sự phân biệt giữa, các ép kiểu *tường minh* so với *ngầm định*. |

Tại sao có ngoại lệ? JS coi `+ ""` là một ép kiểu *ngầm định*, đó là lý do tại sao khi được kích hoạt với một symbol, một ngoại lệ được ném ra. Tôi nghĩ đó là một bằng chứng khá đanh thép.

Tuy nhiên, như tôi đã đề cập ở đầu chương này, Brendan Eich tán thành `+ ""`[^BrendanToString] là cách *tốt nhất* để ép kiểu các giá trị thành chuỗi. Tôi nghĩ điều đó mang rất nhiều trọng lượng, về việc ông ấy ủng hộ ít nhất một tập hợp con các thực hành ép kiểu *ngầm định*. Quan điểm của ông về ép kiểu *ngầm định* hẳn phải sắc thái hơn một chút so với, "tất cả đều xấu".

### Chuyển sang số (To Number)

Các ép kiểu số phức tạp hơn một chút so với ép kiểu chuỗi, vì chúng ta có thể nói về `number` hoặc `bigint` là kiểu đích. Cũng có một tập hợp giá trị nhỏ hơn nhiều có thể được biểu diễn một cách hợp lệ dưới dạng số (mọi thứ khác trở thành `NaN`).

Hãy bắt đầu với các hàm `Number(..)` và `BigInt(..)` (không có từ khóa `new`):

```js
Number("42");                   // 42
Number("-3.141596");            // -3.141596
Number("-0");                   // -0

BigInt("42");                   // 42n
BigInt("-0");                   // 0n
```

Ép kiểu `Number` thất bại (không được nhận ra) dẫn đến `NaN` (xem "Số không hợp lệ" trong Chương 1), trong khi `BigInt` ném ra một ngoại lệ:

```js
Number("123px");                // NaN

BigInt("123px");
// SyntaxError: Cannot convert 123px to a BigInt
```

Hơn nữa, mặc dù `42n` là cú pháp hợp lệ như một `bigint` literal, chuỗi `"42n"` không bao giờ là một biểu diễn chuỗi được nhận ra của một `bigint`, bởi bất kỳ dạng hàm ép kiểu nào:

```js
Number("42n");                  // NaN

BigInt("42n");
// SyntaxError: Cannot convert 42n to a BigInt
```

Tuy nhiên, chúng ta *có thể* ép kiểu các chuỗi số với các biểu diễn khác của các số so với cơ số 10 điển hình (xem Chương 1 để biết thêm thông tin):

```js
Number("0b101010");             // 42

BigInt("0b101010");             // 42n
```

Thông thường, `Number(..)` và `BigInt(..)` nhận các giá trị chuỗi, nhưng điều đó thực sự không bắt buộc. Ví dụ, `true` và `false` ép kiểu thành các tương đương số điển hình của chúng:

```js
Number(true);                   // 1
Number(false);                  // 0

BigInt(true);                   // 1n
BigInt(false);                  // 0n
```

Bạn cũng có thể thường ép kiểu giữa các kiểu `number` và `bigint`:

```js
Number(42n);                    // 42
Number(42n ** 1000n);           // Infinity

BigInt(42);                     // 42n
```

Chúng ta cũng có thể sử dụng toán tử một ngôi `+`, thường được cho là ép kiểu giống như hàm `Number(..)`:

```js
+"42";                          // 42
+"0b101010";                    // 42
```

Mặc dù vậy, hãy cẩn thận. Nếu các ép kiểu không an toàn/không hợp lệ theo những cách nhất định, các ngoại lệ sẽ bị ném ra:

```js
BigInt(3.141596);
// RangeError: The number 3.141596 cannot be converted to a BigInt

+42n;
// TypeError: Cannot convert a BigInt value to a number
```

Rõ ràng, `3.141596` không ép kiểu an toàn thành một số nguyên, chưa nói đến một `bigint`.

Nhưng `+42n` ném ra một ngoại lệ là một trường hợp thú vị. Ngược lại, `Number(42n)` hoạt động tốt, vì vậy hơi ngạc nhiên khi `+42n` thất bại.

| WARNING: |
| :--- |
| Sự ngạc nhiên đó đặc biệt rõ ràng vì việc thêm một dấu `+` trước một số thường được cho là chỉ có nghĩa là một "số dương", giống như cách dấu `-` trước một số được cho là có nghĩa là một "số âm". Tuy nhiên, như đã giải thích trong Chương 1, cú pháp số JS (`number` và `bigint`) không công nhận cú pháp nào cho "giá trị âm". Tất cả các literal số được phân tích cú pháp là "dương" theo mặc định. Nếu một dấu `+` hoặc `-` được thêm vào trước, chúng được coi là các toán tử một ngôi được áp dụng đối với số (dương) đã được phân tích cú pháp. |

OK, vậy `+42n` được phân tích cú pháp là `+(42n)`. Nhưng vẫn... tại sao `+` lại ném ra một ngoại lệ ở đây?

Bạn có thể nhớ lại trước đó khi chúng ta đã chỉ ra rằng JS cho phép ép kiểu chuỗi *tường minh* của các giá trị symbol, nhưng không cho phép ép kiểu chuỗi *ngầm định*? Điều tương tự đang diễn ra ở đây. Thiết kế ngôn ngữ JS diễn giải `+` một ngôi trước một giá trị `bigint` là ép kiểu `ToNumber()` *ngầm định* (do đó bị cấm!), nhưng `Number(..)` được diễn giải là ép kiểu `ToNumber()` *tường minh* (do đó được phép!).

Nói cách khác, trái ngược với giả định/khẳng định phổ biến, `Number(..)` và `+` không thể thay thế cho nhau. Tôi nghĩ `Number(..)` là dạng an toàn hơn/đáng tin cậy hơn.

#### Các phép toán học (Mathematical Operations)

Các toán tử toán học (ví dụ: `+`, `-`, `*`, `/`, `%`, và `**`) mong đợi các toán hạng của chúng là số. Nếu bạn sử dụng một cái gì đó không phải `number` với chúng, giá trị đó sẽ được ép kiểu thành một `number` cho các mục đích tính toán toán học.

Tương tự như cách `x + ""` là một thành ngữ để ép kiểu `x` thành một chuỗi, một biểu thức như `x - 0` ép kiểu `x` thành một số một cách an toàn.

| WARNING: |
| :--- |
| `x + 0` không hoàn toàn an toàn như vậy, vì toán tử `+` được nạp chồng (overloaded) để thực hiện nối chuỗi nếu một trong hai toán hạng đã là một chuỗi. Toán tử trừ `-` không được nạp chồng như vậy, vì vậy ép kiểu duy nhất sẽ là thành `number`. Tất nhiên, `x * 1`, `x / 1`, và thậm chí `x ** 1` cũng thường sẽ tương đương về mặt toán học, nhưng chúng ít phổ biến hơn nhiều, và có lẽ nên tránh vì có khả năng gây nhầm lẫn cho người đọc code của bạn. Ngay cả `x % 1` có vẻ như nó nên an toàn, nhưng nó có thể đưa vào sự sai lệch dấu phẩy động (xem "Độ không chính xác dấu phẩy động" trong Chương 2). |

Bất kể toán tử toán học nào được sử dụng, nếu ép kiểu thất bại, kết quả là `NaN`, và tất cả các toán tử này sẽ lan truyền `NaN` ra ngoài dưới dạng kết quả của chúng.

#### Các phép toán Bitwise (Bitwise Operations)

Các toán tử bitwise (ví dụ: `|`, `&`, `^`, `>>`, `<<`, và `<<<`) đều mong đợi các toán hạng số, nhưng cụ thể chúng kẹp (clamp) các giá trị này thành các số nguyên 32-bit.

Nếu bạn chắc chắn rằng các số bạn đang xử lý nằm an toàn trong phạm vi số nguyên 32-bit, `x | 0` là một thành ngữ biểu thức phổ biến khác có tác dụng ép kiểu `x` thành một `number` nếu cần thiết.

Hơn nữa, vì các engine JS biết các giá trị này sẽ là số nguyên, có khả năng chúng tối ưu hóa cho toán học chỉ số nguyên nếu chúng thấy `x | 0`. Đây là một trong một số "chú thích kiểu" được khuyến nghị từ các nỗ lực ASM.js[^ASMjs] từ nhiều năm trước.

#### Truy cập thuộc tính (Property Access)

Truy cập thuộc tính của đối tượng (và truy cập chỉ mục của mảng) là một nơi khác mà ép kiểu ngầm định có thể xảy ra.

Hãy xem xét:

```js
myObj = {};

myObj[3] = "hello";
myObj["3"] = "world";

console.log( myObj );
```

Bạn mong đợi gì từ nội dung của đối tượng này? Bạn có mong đợi hai thuộc tính khác nhau, số `3` (giữ `"hello"`) và chuỗi `"3"` (giữ `"world"`)? Hay bạn nghĩ cả hai thuộc tính đều ở cùng một vị trí?

Nếu bạn thử code đó, bạn sẽ thấy rằng thực sự chúng ta nhận được một đối tượng với một thuộc tính duy nhất, và nó giữ giá trị `"world"`. Điều đó có nghĩa là bên trong JS đang ép kiểu hoặc `3` thành `"3"`, hoặc ngược lại, khi các truy cập thuộc tính đó được thực hiện.

Thú vị là, bảng điều khiển dành cho nhà phát triển (developer console) rất có thể biểu diễn đối tượng đại loại như thế này:

```js
console.log( myObj );
// {3: 'world'}
```

Số `3` ở đó có chỉ ra rằng thuộc tính là một số `3` không? Không hẳn. Hãy thử thêm một thuộc tính khác vào `myObj`:

```js
myObj.something = 42;

console.log( myObj )
// {3: 'world', something: 42}
```

Chúng ta có thể thấy rằng bảng điều khiển dành cho nhà phát triển này không trích dẫn (quote) các khóa thuộc tính chuỗi, vì vậy chúng ta không thể suy luận bất cứ điều gì từ `3` so với việc nếu bảng điều khiển đã sử dụng `"3"` cho tên khóa.

Thay vào đó, hãy thử tham khảo đặc tả cho giá trị đối tượng[^ObjectValue], nơi chúng ta tìm thấy:

> Một giá trị khóa thuộc tính (property key value) là một giá trị Chuỗi ECMAScript hoặc một giá trị Symbol. Tất cả các giá trị Chuỗi và Symbol, bao gồm Chuỗi rỗng, đều hợp lệ làm khóa thuộc tính. Một tên thuộc tính (property name) là một khóa thuộc tính là một giá trị Chuỗi.

OK! Vì vậy, trong JS, các đối tượng chỉ giữ các thuộc tính chuỗi (hoặc symbol). Điều đó hẳn có nghĩa là số `3` được ép kiểu thành chuỗi `"3"`, đúng không?

Trong cùng phần của đặc tả, chúng ta đọc thêm:

> Một chỉ mục số nguyên (integer index) là một khóa thuộc tính có giá trị Chuỗi là một Chuỗi số chính quy (xem 7.1.21) và có giá trị số là +0𝔽 hoặc một Số nguyên dương ≤ 𝔽(253 - 1). Một chỉ mục mảng (array index) là một chỉ mục số nguyên có giá trị số i nằm trong khoảng +0𝔽 ≤ i < 𝔽(232 - 1).

Nếu một khóa thuộc tính (như `"3"`) *trông* giống như một số, nó được coi là một chỉ mục số nguyên. Hmmm... điều đó gần như có vẻ gợi ý ngược lại với những gì chúng ta vừa đặt ra, đúng không?

Tuy nhiên, chúng ta biết từ trích dẫn trước đó rằng các khóa thuộc tính *chỉ* là chuỗi (hoặc symbol). Vì vậy, hẳn là "chỉ mục số nguyên" ở đây không mô tả vị trí thực tế, mà đúng hơn là việc sử dụng có chủ đích của `3` trong code JS, như một "chỉ mục số nguyên" do nhà phát triển thể hiện; JS sau đó vẫn phải thực sự lưu trữ nó tại vị trí của "Chuỗi số chính quy".

Xem xét các nỗ lực sử dụng các kiểu giá trị khác, như `true`, `null`, `undefined`, hoặc thậm chí các không nguyên thủy (các đối tượng khác):

```js
myObj[true] = 100;
myObj[null] = 200;
myObj[undefined] = 300;
myObj[ {a:1} ] = 400;

console.log(myObj);
// {3: 'world', something: 42, true: 100, null: 200,
// undefined: 300, [object Object]: 400}
```

Như bạn có thể thấy, tất cả các kiểu giá trị khác đó đã được ép kiểu thành chuỗi cho các mục đích tên thuộc tính đối tượng.

Nhưng trước khi chúng ta thuyết phục bản thân về cách giải thích này rằng mọi thứ (thậm chí cả số) đều được ép kiểu thành chuỗi, hãy xem một ví dụ về mảng:

```js
myArr = [];

myArr[3] = "hello";
myArr["3"] = "world";

console.log( myArr );
// [empty × 3, 'world']
```

Bảng điều khiển dành cho nhà phát triển có thể sẽ biểu diễn một mảng hơi khác so với một đối tượng thuần túy. Tuy nhiên, chúng ta vẫn thấy rằng mảng này chỉ có giá trị `"world"` duy nhất trong đó, tại vị trí chỉ mục số tương ứng với `3`.

Kiểu đầu ra đó đại loại ngụ ý ngược lại với cách giải thích trước đó của chúng ta: rằng các giá trị của một mảng đang được lưu trữ chỉ tại các vị trí số. Nếu chúng ta thêm một tên thuộc tính chuỗi khác vào `myArr`:

```js
myArr.something = 42;
console.log( myArr );
// [empty × 3, 'world', something: 42]
```

Bây giờ chúng ta thấy rằng bảng điều khiển dành cho nhà phát triển này đại diện cho các vị trí được lập chỉ mục số trong mảng *không có* tên thuộc tính (vị trí), nhưng thuộc tính `something` được đặt tên trong đầu ra.

Cũng đúng là các engine JS như v8 có xu hướng, vì lý do tối ưu hóa hiệu suất, trường hợp đặc biệt các thuộc tính đối tượng là các chuỗi trông giống số như thực sự được lưu trữ ở các vị trí số như thể chúng là mảng. Vì vậy, ngay cả khi chương trình JS hoạt động như thể tên thuộc tính là `"3"`, thực tế bên dưới lớp vỏ, v8 có thể đang xử lý nó như thể được ép kiểu thành `3`!

Chúng ta có thể rút ra điều gì từ tất cả những điều này?

Đặc tả cho chúng ta biết rõ ràng rằng hành vi của các thuộc tính đối tượng là để chúng được xử lý như chuỗi (hoặc symbol). Điều đó có nghĩa là chúng ta có thể giả định rằng việc sử dụng `3` để truy cập một vị trí trên một đối tượng sẽ có hiệu ứng bên trong là ép kiểu tên thuộc tính đó thành `"3"`.

Nhưng với mảng, chúng ta quan sát thấy một loại ngữ nghĩa ngược lại: sử dụng `"3"` làm tên thuộc tính có tác dụng truy cập vị trí `3` được lập chỉ mục số, như thể chuỗi đã được ép kiểu thành số. Nhưng đó chủ yếu chỉ là một hệ quả của thực tế là mảng luôn có xu hướng hoạt động như được lập chỉ mục số, và/hoặc có lẽ là một sự phản ánh của các chi tiết triển khai/tối ưu hóa bên dưới trong engine JS.

Phần quan trọng là, chúng ta cần nhận ra rằng các đối tượng không thể chỉ đơn giản sử dụng bất kỳ giá trị nào làm tên thuộc tính. Nếu nó là bất cứ thứ gì khác ngoài một chuỗi hoặc một số, chúng ta có thể mong đợi rằng *sẽ có* một sự ép kiểu của giá trị đó.

Chúng ta cần mong đợi và lên kế hoạch cho điều đó thay vì cho phép nó làm chúng ta ngạc nhiên với các lỗi sau này!

### Chuyển đổi sang nguyên thủy (To Primitive)

Hầu hết các toán tử trong JS, bao gồm các toán tử mà chúng ta đã thấy với các ép kiểu sang `string` và `number`, được thiết kế để chạy với các giá trị nguyên thủy. Khi bất kỳ toán tử nào trong số này được sử dụng thay vào đó với một giá trị đối tượng, thuật toán trừu tượng `ToPrimitive` (như đã mô tả trước đó) được kích hoạt để ép kiểu đối tượng thành một giá trị nguyên thủy.

Hãy thiết lập một đối tượng mà chúng ta có thể sử dụng để kiểm tra cách các hoạt động khác nhau hoạt động:

```js
spyObject = {
    toString() {
        console.log("toString() invoked!");
        return "10";
    },
    valueOf() {
        console.log("valueOf() invoked!");
        return 42;
    },
};
```

Đối tượng này định nghĩa cả hai phương thức `toString()` và `valueOf()`, và mỗi phương thức trả về một kiểu giá trị khác nhau (`string` so với `number`).

Hãy thử một số thao tác ép kiểu mà chúng ta đã thấy:

```js
String(spyObject);
// toString() invoked!
// "10"

spyObject + "";
// valueOf() invoked!
// "42"
```

Whoa! Tôi cá rằng điều đó đã làm ngạc nhiên một số bạn đọc; nó chắc chắn đã làm tôi ngạc nhiên. Rất phổ biến khi mọi người khẳng định rằng `String(..)` và `+ ""` là các dạng tương đương để kích hoạt thao tác `ToString()`. Nhưng chúng rõ ràng không phải vậy!

Sự khác biệt đến từ *gợi ý* (hint) mà mỗi thao tác cung cấp cho `ToPrimitive()`. `String(..)` rõ ràng cung cấp `"string"` làm *gợi ý*, trong khi thành ngữ `+ ""` không cung cấp *gợi ý* nào (tương tự như *gợi ý* `"number"`). Nhưng đừng bỏ lỡ chi tiết này: mặc dù `+ ""` gọi `valueOf()`, khi nó trả về giá trị nguyên thủy `number` là `42`, giá trị đó sau đó được ép kiểu thành một chuỗi (thông qua `ToString()`), vì vậy chúng ta nhận được `"42"` thay vì `42`.

Hãy tiếp tục:

```js
Number(spyObject);
// valueOf() invoked!
// 42

+spyObject;
// valueOf() invoked!
// 42
```

Ví dụ này ngụ ý rằng `Number(..)` và toán tử một ngôi `+` đều thực hiện cùng một ép kiểu `ToPrimitive()` (với *gợi ý* là `"number"`), trong trường hợp của chúng ta trả về `42`. Vì đó đã là một `number` như được yêu cầu, giá trị đi ra mà không cần thêm gì nữa.

Nhưng điều gì sẽ xảy ra nếu một `valueOf()` trả về một `bigint`?

```js
spyObject2 = {
    valueOf() {
        console.log("valueOf() invoked!");
        return 42n;  // bigint!
    }
};

Number(spyObject2);
// valueOf() invoked!
// 42     <--- nhìn kìa, không phải là bigint!

+spyObject2;
// valueOf() invoked!
// TypeError: Cannot convert a BigInt value to a number
```

Chúng ta đã thấy sự khác biệt này trước đó trong phần "Chuyển sang số" (To Number). JS cho phép một ép kiểu *tường minh* của giá trị bigint `42n` thành giá trị số `42`, nhưng nó không cho phép cái mà nó coi là một hình thức ép kiểu *ngầm định*.

Còn về hàm ép kiểu `BigInt(..)` (không có từ khóa `new`) thì sao?

```js
BigInt(spyObject);
// valueOf() invoked!
// 42n    <--- nhìn kìa, một bigint!

BigInt(spyObject2);
// valueOf() invoked!
// 42n

// *******************************

spyObject3 = {
    valueOf() {
        console.log("valueOf() invoked!");
        return 42.3;
    }
};

BigInt(spyObject3);
// valueOf() invoked!
// RangeError: The number 42.3 cannot be converted to a BigInt
```

Một lần nữa, như chúng ta đã thấy trong phần "Chuyển sang số", `42` có thể được ép kiểu an toàn thành `42n`. Mặt khác, `42.3` không thể được ép kiểu an toàn thành một `bigint`.

Chúng ta đã thấy rằng `toString()` và `valueOf()` được gọi, khác nhau, khi các ép kiểu `string` và `number` / `bigint` nhất định được thực hiện.

#### Không tìm thấy nguyên thủy? (No Primitive Found?)

Nếu `ToPrimitive()` thất bại trong việc tạo ra một giá trị nguyên thủy, một ngoại lệ sẽ được ném ra:

```js
spyObject4 = {
    toString() {
        console.log("toString() invoked!");
        return [];
    },
    valueOf() {
        console.log("valueOf() invoked!");
        return {};
    }
};

String(spyObject4);
// toString() invoked!
// valueOf() invoked!
// TypeError: Cannot convert object to primitive value

Number(spyObject4);
// valueOf() invoked!
// toString() invoked!
// TypeError: Cannot convert object to primitive value
```

Nếu bạn định định nghĩa các ép kiểu sang nguyên thủy tùy chỉnh thông qua `toString()` / `valueOf()`, hãy chắc chắn trả về một nguyên thủy từ ít nhất một trong số chúng!

#### Đối tượng sang Boolean (Object To Boolean)

Còn về các ép kiểu `boolean` của các đối tượng thì sao?

```js
Boolean(spyObject);
// true

!spyObject;
// false

if (spyObject) {
    console.log("if!");
}
// if!

result = spyObject ? "ternary!" : "nope";
// "ternary!"

while (spyObject) {
    console.log("while!");
    break;
}
// while!
```

Mỗi cái này đều đang kích hoạt `ToBoolean()`. Nhưng nếu bạn nhớ lại từ trước, thuật toán *đó* không bao giờ ủy quyền cho `ToPrimitive()`; do đó, chúng ta không thấy "valueOf() invoked!" được log ra.

#### Unboxing: Wrapper sang nguyên thủy

Một dạng đặc biệt của các đối tượng thường được ép kiểu `ToPrimitive()`: các nguyên thủy được đóng hộp/bao bọc (boxed/wrapped) (như đã thấy trong Chương 3). Sự ép kiểu đối tượng-sang-nguyên thủy cụ thể này thường được gọi là *unboxing*.

Hãy xem xét:

```js
hello = new String("hello");
String(hello);                  // "hello"
hello + "";                     // "hello"

fortyOne = new Number(41);
Number(fortyOne);               // 41
fortyOne + 1;                   // 42
```

Các wrapper đối tượng `hello` và `fortyOne` ở trên có các phương thức `toString()` và `valueOf()` được định cấu hình trên chúng, để hoạt động tương tự như các đối tượng `spyObject` / v.v. từ các ví dụ trước của chúng ta.

Một trường hợp đặc biệt cần cẩn thận với các nguyên thủy wrapped-object là với `Boolean()`:

```js
nope = new Boolean(false);
Boolean(nope);                  // true   <--- oops!
!!nope;                         // true   <--- oops!
```

Hãy nhớ rằng, điều này là do `ToBoolean()` *không* giảm một đối tượng xuống dạng nguyên thủy của nó với `ToPrimitive`; nó chỉ đơn thuần tra cứu giá trị trong bảng nội bộ của nó, và vì các đối tượng bình thường (không exotic[^ExoticFalsyObjects]) luôn luôn là truthy, `true` được đưa ra.

| NOTE: |
| :--- |
| Đó là một cạm bẫy nhỏ khó chịu. Chắc chắn có thể đưa ra một lập luận rằng `new Boolean(false)` nên tự định cấu hình bên trong như một "đối tượng falsy" exotic. [^ExoticFalsyObjects] Thật không may, sự thay đổi đó bây giờ, 25 năm vào lịch sử của JS, có thể dễ dàng gây ra sự cố trong các chương trình. Như vậy, JS đã để cạm bẫy này không bị chạm tới. |

#### Ghi đè `toString()` mặc định

Như chúng ta đã thấy, bạn luôn có thể định nghĩa một `toString()` trên một đối tượng để *nó* được gọi bởi ép kiểu `ToPrimitive()` thích hợp. Nhưng một tùy chọn khác là ghi đè `Symbol.toStringTag`:

```js
spyObject5a = {};
String(spyObject5a);
// "[object Object]"
spyObject5a.toString();
// "[object Object]"

spyObject5b = {
    [Symbol.toStringTag]: "my-spy-object"
};
String(spyObject5b);
// "[object my-spy-object]"
spyObject5b.toString();
// "[object my-spy-object]"

spyObject5c = {
    get [Symbol.toStringTag]() {
        return `myValue:${this.myValue}`;
    },
    myValue: 42
};
String(spyObject5c);
// "[object myValue:42]"
spyObject5c.toString();
// "[object myValue:42]"
```

`Symbol.toStringTag` được dự định để định nghĩa một giá trị chuỗi tùy chỉnh để mô tả đối tượng bất cứ khi nào thao tác `toString()` mặc định của nó được gọi trực tiếp, hoặc ngầm định qua ép kiểu; khi vắng mặt nó, giá trị được sử dụng là `"Object"` trong đầu ra `"[object Object]"` phổ biến.

Cú pháp `get ..` trong `spyObject5c` đang định nghĩa một *getter*. Điều đó có nghĩa là khi JS cố gắng truy cập `Symbol.toStringTag` này như một thuộc tính (như bình thường), code getter này thay vào đó khiến hàm chúng ta chỉ định được gọi để tính toán kết quả. Chúng ta có thể chạy bất kỳ logic tùy ý nào bên trong getter này để xác định động một *thẻ* (tag) chuỗi để phương thức `toString()` mặc định sử dụng.

#### Ghi đè `ToPrimitive`

Bạn có thể thay thế ghi đè toàn bộ thao tác `ToPrimitive()` mặc định cho bất kỳ đối tượng nào, bằng cách đặt thuộc tính symbol đặc biệt `Symbol.toPrimitive` để giữ một hàm:

```js
spyObject6 = {
    [Symbol.toPrimitive](hint) {
        console.log(`toPrimitive(${hint}) invoked!`);
        return 25;
    },
    toString() {
        console.log("toString() invoked!");
        return "10";
    },
    valueOf() {
        console.log("valueOf() invoked!");
        return 42;
    },
};

String(spyObject6);
// toPrimitive(string) invoked!
// "25"   <--- không phải "10"

spyObject6 + "";
// toPrimitive(default) invoked!
// "25"   <--- không phải "42"

Number(spyObject6);
// toPrimitive(number) invoked!
// 25     <--- không phải 42 hay "25"

+spyObject6;
// toPrimitive(number) invoked!
// 25
```

Như bạn có thể thấy, nếu bạn định nghĩa hàm này trên một đối tượng, nó được sử dụng hoàn toàn thay thế cho thao tác trừu tượng `ToPrimitive()` mặc định. Vì `hint` vẫn được cung cấp cho hàm được gọi này (`[Symbol.toPrimitive](..)`), về lý thuyết bạn có thể triển khai phiên bản thuật toán của riêng mình, gọi một `toString()`, `valueOf()`, hoặc bất kỳ phương thức nào khác trên đối tượng (tham chiếu ngữ cảnh `this`).

Hoặc bạn chỉ có thể định nghĩa thủ công một giá trị trả về như hình trên. Bất kể thế nào, JS sẽ *không* tự động gọi các phương thức `toString()` hoặc `valueOf()`.

| WARNING: |
| :--- |
| Như đã thảo luận trước đó trong "Không tìm thấy nguyên thủy?", nếu hàm `Symbol.toPrimitive` được định nghĩa không thực sự trả về một giá trị là nguyên thủy, một ngoại lệ sẽ được ném ra về việc không thể "...chuyển đổi đối tượng thành giá trị nguyên thủy" (...convert object to primitive value). Hãy chắc chắn luôn trả về một giá trị nguyên thủy thực tế từ một hàm như vậy! |

### So sánh bằng (Equality)

Cho đến nay, các ép kiểu mà chúng ta đã thấy tập trung vào các giá trị đơn lẻ. Bây giờ chúng ta chuyển sự chú ý sang các so sánh bằng, vốn dĩ liên quan đến hai giá trị, một hoặc cả hai giá trị đó có thể chịu sự ép kiểu.

Trước đó trong chương này, chúng ta đã nói về một số thao tác trừu tượng để so sánh bằng giá trị.

Ví dụ, thao tác `SameValue()`[^SameValue] là phép so sánh bằng nghiêm ngặt nhất, hoàn toàn không có ép kiểu. Thao tác JS rõ ràng nhất dựa trên `SameValue()` là:

```js
Object.is(42,42);                   // true
Object.is(-0,-0);                   // true
Object.is(NaN,NaN);                 // true

Object.is(0,-0);                    // false
```

Thao tác `SameValueZero()` -- hãy nhớ rằng, nó chỉ khác với `SameValue()` ở chỗ coi `-0` và `0` là không thể phân biệt được -- được sử dụng ở khá nhiều nơi khác, bao gồm:

```js
[ 1, 2, NaN ].includes(NaN);        // true
```

Chúng ta có thể thấy sự sai hướng `0` / `-0` của `SameValueZero()` ở đây:

```js
[ 1, 2, -0 ].includes(0);           // true  <--- oops!

(new Set([ 1, 2, 0 ])).has(-0);     // true  <--- ugh

(new Map([[ 0, "ok" ]])).has(-0);   // true  <--- :(
```

Trong những trường hợp này, có một sự *ép kiểu* (đại loại vậy!) coi `-0` và `0` là không thể phân biệt được. Không, về mặt kỹ thuật, đó không phải là một "sự ép kiểu" ở chỗ kiểu không bị thay đổi, nhưng tôi đang hơi lạm dụng định nghĩa để *bao gồm* trường hợp này trong cuộc thảo luận rộng hơn của chúng ta về ép kiểu ở đây.

So sánh các phương thức `includes()` / `has()` ở đây, kích hoạt `SameValueZero()`, với tiện ích mảng `indexOf(..)` cũ kỹ, thay vào đó kích hoạt `IsStrictlyEqual()`. Thuật toán này "ép kiểu" hơn một chút so với `SameValueZero()`, ở chỗ nó ngăn các giá trị `NaN` bao giờ được coi là bằng nhau:

```js
[ 1, 2, NaN ].indexOf(NaN);         // -1  <--- không tìm thấy
```

Nếu những điều kỳ quặc đầy sắc thái này của `includes(..)` và `indexOf(..)` làm phiền bạn, khi tìm kiếm -- tìm kiếm một kết quả khớp bằng nhau bên trong -- cho một giá trị trong một mảng, bạn có thể tránh bất kỳ sự "ép kiểu" kỳ quặc nào và *buộc* khớp bằng nhau `SameValue()` nghiêm ngặt nhất, thông qua `Object.is(..)`:

```js
vals = [ 0, 1, 2, -0, NaN ];

vals.find(v => Object.is(v,-0));            // -0
vals.find(v => Object.is(v,NaN));           // NaN

vals.findIndex(v => Object.is(v,-0));       // 3
vals.findIndex(v => Object.is(v,NaN));      // 4
```

#### Các toán tử so sánh bằng: `==` vs `===`

Nơi rõ ràng nhất mà *ép kiểu* có liên quan trong các kiểm tra so sánh bằng là với toán tử `==`. Bất chấp mọi định kiến bạn có thể có về `==`, nó hoạt động cực kỳ dễ đoán, đảm bảo rằng cả hai toán hạng khớp kiểu trước khi thực hiện kiểm tra so sánh bằng của nó.

Để khẳng định một điều có thể hoặc không quá rõ ràng: các toán tử `==` (và `===`) luôn trả về một `boolean` (`true` hoặc `false`), biểu thị kết quả của kiểm tra so sánh bằng; chúng không bao giờ trả về bất kỳ thứ gì khác, bất kể ép kiểu nào có thể xảy ra.

Bây giờ, hãy nhớ lại và xem xét các bước đã thảo luận trước đó trong chương về thao tác `IsLooselyEqual()`. [^LooseEquality] Hành vi của nó, và do đó cách `==` hoạt động, có thể được trực giác hóa một cách thực tế chỉ với hai sự thật này trong đầu:

1. Nếu kiểu của cả hai toán hạng giống nhau, `==` có hành vi giống hệt như `===` -- `IsLooselyEqual()` ngay lập tức ủy quyền cho `IsStrictlyEqual()`. [^StrictEquality]

    Ví dụ, khi cả hai toán hạng đều là tham chiếu đối tượng:

    ```js
    myObj = { a: 1 };
    anotherObj = myObj;

    myObj == anotherObj;                // true
    myObj === anotherObj;               // true
    ```

    Ở đây, `==` và `===` xác định rằng cả hai toán hạng tương ứng của chúng đều thuộc kiểu tham chiếu `object`, vì vậy cả hai kiểm tra so sánh bằng đều hoạt động giống hệt nhau; chúng so sánh các tham chiếu đối tượng để xem có bằng nhau không.

2. Nhưng nếu các kiểu toán hạng khác nhau, `==` cho phép ép kiểu cho đến khi chúng khớp nhau, và ưu tiên so sánh số; nó cố gắng ép kiểu cả hai toán hạng thành số, nếu có thể:

    ```js
    42 == "42";                         // true
    ```

    Ở đây, chuỗi `"42"` được ép kiểu thành số `42` (không phải ngược lại), và do đó phép so sánh sau đó là `42 == 42`, và rõ ràng phải trả về `true`.


Được trang bị kiến thức này, bây giờ chúng ta sẽ xua tan huyền thoại phổ biến rằng chỉ có `===` kiểm tra kiểu và giá trị, trong khi `==` chỉ kiểm tra giá trị. Không đúng!

Trên thực tế, `==` và `===` đều nhạy cảm với kiểu, mỗi toán tử đều kiểm tra kiểu của các toán hạng của chúng. Toán tử `==` cho phép ép kiểu các kiểu không khớp, trong khi `===` không cho phép bất kỳ ép kiểu nào.

Đó là một ý kiến được tin tưởng gần như phổ biến rằng `==` nên tránh để ủng hộ `===`. Tôi có thể là một trong số ít các nhà phát triển công khai ủng hộ một trường hợp rõ ràng và nghiêm túc cho điều ngược lại. Tôi nghĩ lý do chính mà mọi người thay vào đó thích `===`, ngoài việc đơn giản là tuân theo hiện trạng, là do thiếu thời gian để thực sự hiểu `==`.

Tôi sẽ xem xét lại chủ đề này để đưa ra lập luận ủng hộ `==` hơn là `===`, sau đó trong chương này, trong phần "So sánh bằng có nhận thức về kiểu" (Type Aware Equality). Tất cả những gì tôi yêu cầu là, bất kể bạn hiện đang không đồng ý với tôi mạnh mẽ đến mức nào, hãy cố gắng giữ một tư duy cởi mở.

#### Ép kiểu Nullish (Nullish Coercion)

Chúng ta đã thấy một số thao tác JS có tính chất nullish -- coi `null` và `undefined` là tương đương về mặt ép kiểu với nhau, bao gồm toán tử chuỗi tùy chọn `?.` và toán tử kết hợp nullish `??` (xem "Null'ish" trong Chương 1).

Nhưng `==` là nơi rõ ràng nhất mà JS phơi bày sự bằng nhau ép kiểu nullish:

```js
null == undefined;              // true
```

Cả `null` và `undefined` sẽ không bao giờ tương đương về mặt ép kiểu với bất kỳ giá trị nào khác trong ngôn ngữ, ngoài chính chúng. Điều đó có nghĩa là `==` làm cho việc coi hai giá trị này là không thể phân biệt được trở nên thuận tiện.

Bạn có thể tận dụng khả năng này như sau:

```js
if (someData == null) {
    // `someData` là "unset" (null hoặc undefined),
    // vì vậy hãy đặt nó thành một giá trị mặc định nào đó
}

// HOẶC:

if (someData != null) {
    // `someData` được đặt (không phải null cũng không phải undefined),
    // vì vậy hãy sử dụng nó theo cách nào đó
}
```

Hãy nhớ rằng `!=` là phủ định của `==`, trong khi `!==` là phủ định của `===`. Đừng khớp số lượng dấu `=` trừ khi bạn muốn làm mình bối rối!

So sánh hai cách tiếp cận này:

```js
if (someData == null) {
    // ..
}

// so với:

if (someData === null || someData === undefined) {
    // ..
}
```

Cả hai câu lệnh `if` sẽ hoạt động hoàn toàn giống hệt nhau. Bạn muốn viết cái nào hơn, và bạn muốn đọc cái nào hơn sau này?

Công bằng mà nói, một số bạn thích sự tương đương `===` dài dòng hơn. Và điều đó ổn. Tôi không đồng ý, tôi nghĩ phiên bản `==` của kiểm tra này tốt hơn *nhiều*. Và tôi cũng duy trì rằng phiên bản `==` nhất quán hơn về tinh thần phong cách với cách các toán tử nullish khác như `?.` và `??` hoạt động.

Nhưng một thực tế nhỏ khác mà bạn có thể xem xét: trong các điểm chuẩn hiệu suất mà tôi đã chạy nhiều lần, các engine JS có thể thực hiện kiểm tra `== null` đơn lẻ như được hiển thị *nhanh hơn một chút* so với sự kết hợp của hai kiểm tra `===`. Nói cách khác, có một lợi ích nhỏ nhưng có thể đo lường được khi để `==` của JS thực hiện ép kiểu nullish *ngầm định* hơn là cố gắng liệt kê *tường minh* cả hai kiểm tra chính mình.

Tôi quan sát thấy rằng ngay cả nhiều người hâm mộ `===` cứng đầu cũng có xu hướng thừa nhận rằng `== null` ít nhất là một trường hợp như vậy mà `==` được ưu tiên hơn.

#### Cạm bẫy Boolean của `==`

Ngoài một số trường hợp góc ép kiểu mà chúng ta sẽ giải quyết trong phần tiếp theo, có lẽ cạm bẫy lớn nhất cần biết với `==` liên quan đến boolean.

Hãy chú ý rất kỹ ở đây, vì đó là một trong những lý do lớn nhất khiến mọi người bị cắn, và sau đó trở nên coi thường `==`. Nếu bạn làm theo lời khuyên đơn giản của tôi (ở cuối phần này), bạn sẽ không bao giờ trở thành nạn nhân!

Hãy xem xét đoạn mã sau, và hãy giả sử trong một phút rằng `isLoggedIn` *không* giữ một giá trị `boolean` (`true` hoặc `false`):

```js
if (isLoggedIn) {
    // ..
}

// so với:

if (isLoggedIn == true) {
    // ..
}
```

Chúng ta đã đề cập đến dạng câu lệnh `if` đầu tiên. Chúng ta biết `if` mong đợi một `boolean`, vì vậy trong trường hợp này `isLoggedIn` sẽ được ép kiểu thành một `boolean` bằng cách sử dụng bảng tra cứu trong thao tác trừu tượng `ToBoolean()`. Khá đơn giản để dự đoán, phải không?

Nhưng hãy xem xét biểu thức `isLoggedIn == true`. Bạn có nghĩ rằng nó sẽ hoạt động theo cùng một cách không?

Nếu bản năng của bạn là *có*, bạn vừa rơi vào một cái bẫy nhỏ lắt léo. Hãy nhớ lại đầu chương này khi tôi cảnh báo rằng các quy tắc của ép kiểu `ToBoolean()` chỉ áp dụng nếu thao tác JS thực sự kích hoạt thuật toán đó. Ở đây, có vẻ như JS phải đang làm như vậy, bởi vì `== true` có vẻ rất rõ ràng là một loại so sánh "liên quan đến boolean".

Nhưng không. Hãy đọc lại thuật toán `IsLooselyEqual()` (cho `==`) trước đó trong chương này. Đi nào, tôi sẽ đợi. Nếu bạn không thích bản tóm tắt của tôi, hãy đọc chính thuật toán đặc tả[^LooseEquality].

OK, bạn có thấy bất cứ điều gì trong đó đề cập đến việc gọi `ToBoolean()` trong bất kỳ trường hợp nào không?

Không!

Hãy nhớ rằng: khi các kiểu của hai toán hạng `==` không giống nhau, nó ưu tiên ép kiểu cả hai thành số.

Cái gì có thể có trong `isLoggedIn`, nếu nó không phải là một `boolean`? Chà, nó có thể là một giá trị chuỗi như `"yes"`, chẳng hạn. Ở dạng đó, `if ("yes") { .. }` rõ ràng sẽ vượt qua kiểm tra điều kiện và thực thi khối lệnh.

Nhưng điều gì sẽ xảy ra với dạng `==` của điều kiện `if`? Nó sẽ hoạt động như thế này:

```js
// (1)
"yes" == true

// (2)
"yes" == 1

// (3)
NaN == 1

// (4)
NaN === 1           // false
```

Nói cách khác, nếu `isLoggedIn` giữ một giá trị như `"yes"`, khối `if (isLoggedIn) { .. }` sẽ vượt qua kiểm tra điều kiện, nhưng kiểm tra `if (isLoggedIn == true)` sẽ không. Ugh!

Điều gì sẽ xảy ra nếu `isLoggedIn` giữ chuỗi `"true"`?

```js
// (1)
"true" == true

// (2)
"true" == 1

// (3)
NaN == 1

// (4)
NaN === 1           // false
```

Vỗ trán (Facepalm).

Đây là một câu đố nhanh: giá trị nào `isLoggedIn` cần giữ để cả hai dạng điều kiện câu lệnh `if` đều vượt qua?

...

...

...

...

Điều gì sẽ xảy ra nếu `isLoggedIn` đang giữ số `1`? `1` là truthy, vì vậy dạng `if (isLoggedIn)` vượt qua. Và dạng `==` khác liên quan đến ép kiểu:

```js
// (1)
1 == true

// (2)
1 == 1

// (3)
1 === 1             // true
```

Nhưng nếu `isLoggedIn` thay vào đó giữ chuỗi `"1"`? Một lần nữa, `"1"` là truthy, nhưng còn về ép kiểu `==` thì sao?

```js
// (1)
"1" == true

// (2)
"1" == 1

// (3)
1 == 1

// (4)
1 === 1             // true
```

OK, vậy `1` và `"1"` là hai giá trị mà `isLoggedIn` có thể giữ an toàn để ép kiểu cùng với `true` trong một kiểm tra so sánh bằng `==`. Nhưng về cơ bản, hầu như không có giá trị nào khác an toàn cho `isLoggedIn` giữ.

Chúng ta có một cạm bẫy tương tự nếu kiểm tra là `== false`. Những giá trị nào an toàn trong một so sánh như vậy? `""` và `0` hoạt động. Nhưng:

```js
if ([] == false) {
    // cái này sẽ chạy!
}
```

`[]` là một giá trị truthy, nhưng nó cũng tương đương về mặt ép kiểu với `false`?! Ouch.

Chúng ta phải làm gì với những cạm bẫy này với các kiểm tra `== true` và `== false`? Tôi có một câu trả lời rõ ràng và đơn giản.

Không bao giờ, không bao giờ, trong bất kỳ trường hợp nào, thực hiện kiểm tra `==` nếu một trong hai bên của phép so sánh là giá trị `true` hoặc `false`. Có vẻ như nó sẽ hoạt động như một ép kiểu `ToBoolean()` tốt đẹp, nhưng nó sẽ khôn khéo không làm vậy, và thay vào đó sẽ bị vướng vào nhiều trường hợp góc ép kiểu (được giải quyết trong phần tiếp theo). Và cũng tránh các dạng `===`.

Khi bạn đang làm việc với boolean, hãy gắn bó với các dạng ép kiểu ngầm định thực sự kích hoạt `ToBoolean()`, chẳng hạn như `if (isLoggedIn)`, và tránh xa các dạng `==` / `===`.

## Các trường hợp góc của ép kiểu (Coercion Corner Cases)

Tôi đã rõ ràng trong việc bày tỏ quan điểm ủng hộ ép kiểu của mình cho đến nay. Và đó *chỉ* là một ý kiến, mặc dù nó dựa trên việc diễn giải các sự kiện thu thập được từ việc nghiên cứu đặc tả ngôn ngữ và các hành vi có thể quan sát được của JS.

Điều đó không có nghĩa là ép kiểu là hoàn hảo. Có một số trường hợp góc gây nản lòng mà chúng ta cần phải nhận thức được, để chúng ta tránh vấp phải những ổ gà đó. Trong trường hợp chưa rõ ràng, những mô tả sau đây của tôi về các trường hợp góc này chỉ là thêm những ý kiến ​​của tôi. Trải nghiệm của bạn có thể khác.

### Chuỗi (Strings)

Chúng ta đã thấy rằng ép kiểu chuỗi của một mảng trông như thế này:

```js
String([ 1, 2, 3 ]);                // "1,2,3"
```

Cá nhân tôi thấy điều đó cực kỳ khó chịu, rằng nó không bao gồm các dấu `[ ]` bao quanh. Đặc biệt, điều đó dẫn đến sự vô lý này:

```js
String([]);                         // ""
```

Vì vậy, chúng ta thậm chí không thể biết rằng đó là một mảng, bởi vì tất cả những gì chúng ta nhận được là một chuỗi rỗng? Tuyệt vời, JS. Điều đó thật ngu ngốc. Xin lỗi, nhưng đúng là như vậy. Và nó còn tồi tệ hơn:

```js
String([ null, undefined ]);        // ","
```

CÁI QUÁI GÌ VẬY!? Chúng ta biết rằng `null` ép kiểu thành chuỗi `"null"`, và `undefined` ép kiểu thành chuỗi `"undefined"`. Nhưng nếu những giá trị đó nằm trong một mảng, chúng chỉ *biến mất* một cách kỳ diệu thành các chuỗi rỗng trong quá trình ép kiểu mảng thành chuỗi. Chỉ còn lại dấu `","` để gợi ý cho chúng ta rằng có bất cứ thứ gì trong mảng! Đó chỉ là chuyện ngớ ngẩn, ngay tại đó.

Còn các đối tượng thì sao? Gần như cũng gây khó chịu, mặc dù theo hướng ngược lại:

```js
String({});                         // "[object Object]"

String({ a: 1 });                   // "[object Object]"
```

Umm... OK. Chắc chắn rồi, cảm ơn JS vì không giúp ích gì cả trong việc hiểu giá trị đối tượng là gì.

### Số (Numbers)

Tôi sắp tiết lộ những gì tôi nghĩ là gốc rễ tồi tệ nhất của mọi tội lỗi trường hợp góc ép kiểu. Bạn đã sẵn sàng chưa?!?

```js
Number("");                         // 0
Number("       ");                  // 0
```

Tôi vẫn lắc đầu về điều này, và tôi đã biết về nó trong gần 20 năm. Tôi vẫn không hiểu Brendan đã nghĩ gì với cái này.

Chuỗi rỗng không có bất kỳ nội dung nào; nó không có gì trong đó để xác định một biểu diễn số. `0` hoàn toàn ***KHÔNG*** phải là tương đương số của giá trị số bị thiếu/không hợp lệ. Bạn có biết giá trị số nào chúng ta có rất phù hợp để giao tiếp điều đó không? `NaN`. Đừng thậm chí bắt đầu với tôi về cách khoảng trắng bị loại bỏ khỏi chuỗi khi ép kiểu thành một số, vì vậy chuỗi `"       "` rất-không-phải-là-rỗng vẫn được xử lý giống như `""` cho các mục đích ép kiểu số.

Tệ hơn nữa, hãy nhớ lại `[]` ép kiểu thành chuỗi `""` như thế nào? Bằng cách mở rộng:

```js
Number([]);                         // 0
```

Doh! Nếu `""` không ép kiểu thành `0` -- hãy nhớ rằng, đây là gốc rễ của mọi tội lỗi ép kiểu! --, thì `[]` cũng sẽ không ép kiểu thành `0`.

Đây chỉ là lãnh thổ vũ trụ lộn ngược vô lý.

Dễ chịu hơn nhiều, nhưng vẫn hơi khó chịu:

```js
Number("NaN");                      // NaN  <--- tình cờ!

Number("Infinity");                 // Infinity
Number("infinity");                 // NaN  <--- oops, chú ý chữ hoa thường!
```

Chuỗi `"NaN"` không được phân tích thành một giá trị số có thể nhận ra, vì vậy quá trình ép kiểu thất bại, tạo ra (vô tình!) giá trị `NaN`. `"Infinity"` có thể phân tích cú pháp một cách rõ ràng cho việc ép kiểu, nhưng bất kỳ cách viết hoa thường nào khác, bao gồm `"infinity"`, sẽ thất bại, lại tạo ra `NaN`.

Ví dụ tiếp theo này, bạn có thể không nghĩ đó là một trường hợp góc chút nào:

```js
Number(false);                      // 0
Number(true);                       // 1
```

Đó chỉ đơn thuần là quy ước của lập trình viên, di sản từ các ngôn ngữ ban đầu không có giá trị boolean `true` và `false`, mà chúng ta coi `0` là `false`, và `1` là `true`. Nhưng liệu có *thực sự* hợp lý khi đi theo hướng ngược lại không?

Hãy nghĩ về nó theo cách này:

```js
false + true + false + false + true;        // 2
```

Thật sao? Tôi không nghĩ có bất kỳ trường hợp nào mà việc coi một `boolean` như tương đương `number` của nó có ý nghĩa hợp lý trong một chương trình. Tôi có thể hiểu chiều ngược lại, vì lý do lịch sử: `Boolean(0)` và `Boolean(1)`.

Nhưng tôi thực sự cảm thấy rằng `Number(false)` và `Number(true)` (cũng như bất kỳ hình thức ép kiểu ngầm định nào) nên tạo ra `NaN`, không phải `0` / `1`.

### Sự vô lý của ép kiểu (Coercion Absurdity)

Để chứng minh quan điểm của tôi, hãy đưa sự vô lý lên cấp độ 11:

```js
[] == ![];                          // true
```

Làm sao vậy!? Điều đó có vẻ vượt quá sự tin cậy rằng một giá trị có thể tương đương về mặt ép kiểu với phủ định của nó, phải không!?

Nhưng hãy đi xuống hang thỏ ép kiểu:

1. `[] == ![]`
2. `[] == false`
3. `"" == false`
4. `0 == false`
5. `0 == 0`
6. `0 === 0`  ->  `true`

Chúng ta có ba sự vô lý khác nhau âm mưu chống lại chúng ta: `String([])`, `Number("")`, và `Number(false)`; nếu bất kỳ điều nào trong số này không đúng, kết quả trường hợp góc vô nghĩa này sẽ không xảy ra.

Tuy nhiên, hãy để tôi làm rõ điều gì đó hoàn toàn: không có điều nào trong số này là lỗi của `==`. Tất nhiên, nó bị đổ lỗi ở đây. Nhưng thủ phạm thực sự là các trường hợp góc `string` và `number` cơ bản.

## Nhận thức về kiểu (Type Awareness)

Bây giờ chúng ta đã mổ xẻ và kiểm tra ép kiểu từ mọi góc độ có thể hình dung, bắt đầu từ những nội dung trừu tượng của đặc tả, sau đó chuyển sang các biểu thức và câu lệnh cụ thể thực sự kích hoạt các ép kiểu.

Nhưng mục đích của tất cả những điều này là gì? Có phải chi tiết trong chương này, và thực sự là cả cuốn sách này cho đến thời điểm này, chủ yếu chỉ là những chuyện vặt vãnh? Eh, tôi không nghĩ vậy.

Hãy quay lại những quan sát/câu hỏi mà tôi đã đặt ra ngay từ đầu chương dài này.

Không thiếu những ý kiến ​​(đặc biệt là tiêu cực) về ép kiểu. Quan điểm gần như phổ biến là ép kiểu chủ yếu/hoàn toàn là một *phần tồi tệ* trong thiết kế ngôn ngữ của JS. Nhưng bất chấp thực tế đó, hầu hết mọi nhà phát triển, trong hầu hết mọi chương trình JS từng được viết, đều phải đối mặt với thực tế là không thể tránh khỏi việc ép kiểu.

Nói cách khác, cho dù bạn làm gì, bạn sẽ không thể thoát khỏi nhu cầu phải nhận thức, hiểu và quản lý các kiểu giá trị của JS và các chuyển đổi của chúng. Trái ngược với những giả định thông thường, việc chấp nhận một ngôn ngữ định kiểu động (hoặc thậm chí là định kiểu yếu), *không* có nghĩa là bất cẩn hoặc không nhận thức về kiểu.

Lập trình có nhận thức về kiểu luôn luôn, luôn tốt hơn lập trình thiếu hiểu biết/bất cần về kiểu.

### Uhh... TypeScript?

Chắc chắn bạn đang nghĩ ngay lúc này: "Tại sao tôi không thể chỉ sử dụng TypeScript và khai báo tất cả các kiểu của mình một cách tĩnh, tránh mọi sự nhầm lẫn của định kiểu động và ép kiểu?"

| NOTE: |
| :--- |
| Tôi có nhiều suy nghĩ chi tiết hơn về TypeScript và vai trò lớn hơn mà nó đóng trong hệ sinh thái của chúng ta; Tôi sẽ dành những ý kiến ​​đó cho phần phụ lục ("Suy nghĩ về TypeScript"). |

Hãy bắt đầu bằng cách giải quyết trực tiếp các cách mà TypeScript có và không hỗ trợ trong lập trình có nhận thức về kiểu, như tôi đang ủng hộ.

TypeScript vừa là **định kiểu tĩnh** (statically-typed - nghĩa là các kiểu được khai báo tại thời điểm viết code và được kiểm tra tại thời điểm biên dịch) và **định kiểu mạnh** (strongly-typed - nghĩa là các biến/thùng chứa được định kiểu và các liên kết này được thực thi; các hệ thống định kiểu mạnh cũng không cho phép ép kiểu *ngầm định*). Điểm mạnh lớn nhất của TypeScript là nó thường buộc cả người viết code và người đọc code phải đối mặt với các kiểu bao gồm hầu hết (tốt nhất là tất cả!) của một chương trình. Đó chắc chắn là một điều tốt.

Ngược lại, JS là **định kiểu động** (dynamically-typed - nghĩa là các kiểu được khám phá và quản lý hoàn toàn tại thời gian chạy) và **định kiểu yếu** (weakly-typed - nghĩa là các biến/thùng chứa không được định kiểu, vì vậy không có liên kết nào để thực thi và do đó các biến có thể chứa bất kỳ kiểu giá trị nào; các hệ thống định kiểu yếu cho phép mọi hình thức ép kiểu).

| NOTE: |
| :--- |
| Tôi đang giải thích sơ qua ở mức độ khá cao ở đây và cố ý không đi sâu vào nhiều sắc thái trên các phổ định kiểu tĩnh/động và mạnh/yếu. Nếu bạn đang cảm thấy thôi thúc muốn "À, thực ra thì..." (Well, actually...) với tôi ngay lúc này, vui lòng đợi một chút và để tôi trình bày lập luận của mình. |

### Nhận thức về kiểu *không cần* TypeScript

Liệu một hệ thống định kiểu động có tự động có nghĩa là bạn đang lập trình với ít nhận thức về kiểu hơn không? Nhiều người sẽ lập luận điều đó, nhưng tôi không đồng ý.

Tôi hoàn toàn không nghĩ rằng việc khai báo các kiểu tĩnh (chú thích, như trong TypeScript) là cách duy nhất để đạt được nhận thức về kiểu hiệu quả. Tuy nhiên, rõ ràng là những người ủng hộ định kiểu tĩnh tin rằng đó là cách *tốt nhất*.

Hãy để tôi minh họa nhận thức về kiểu mà không cần định kiểu tĩnh của TypeScript. Hãy xem xét khai báo biến này:

```js
let API_BASE_URL = "https://some.tld/api/2";
```

Câu lệnh đó có theo bất kỳ cách nào là *có nhận thức về kiểu* không? Chắc chắn, không có chú thích `: string` nào sau `API_BASE_URL`. Nhưng tôi chắc chắn nghĩ rằng nó *vẫn* là có nhận thức về kiểu! Chúng ta thấy rõ kiểu giá trị (`string`) của giá trị được gán cho `API_BASE_URL`.

| WARNING: |
| :--- |
| Đừng bị phân tâm bởi việc khai báo `let` có thể gán lại (trái ngược với `const`). `const` của JS *không phải* là một tính năng hạng nhất của hệ thống kiểu của nó. Chúng ta không thực sự đạt được thêm nhận thức về kiểu chỉ vì chúng ta biết rằng việc gán lại một biến `const` bị engine JS không cho phép. Nếu code được cấu trúc tốt -- e hèm, được cấu trúc với nhận thức về kiểu là ưu tiên -- chúng ta chỉ cần đọc code và thấy rõ rằng `API_BASE_URL` *không* được gán lại và do đó vẫn là kiểu giá trị mà nó đã được gán trước đó. Từ góc độ nhận thức về kiểu, điều đó thực sự giống như thể nó *không thể* được gán lại. |

Nếu sau này tôi muốn làm một cái gì đó như:

```js
// chúng ta có đang sử dụng secure API URL không?
isSecureAPI = /^https/.test(API_BASE_URL);
```

Tôi biết phương thức `test(..)` của biểu thức chính quy yêu cầu một chuỗi, và vì tôi biết `API_BASE_URL` đang giữ một chuỗi, tôi biết thao tác đó là an toàn về kiểu (type-safe).

Tương tự, vì tôi biết các quy tắc đơn giản của ép kiểu `ToBoolean()` liên quan đến các giá trị chuỗi, tôi biết loại câu lệnh này cũng an toàn về kiểu:

```js
// chúng ta đã xác định được API URL chưa?
if (API_BASE_URL) {
    // ..
}
```

Nhưng nếu sau này, tôi bắt đầu gõ một cái gì đó như thế này:

```js
APIVersion = Number(API_BASE_URL);
```

Một tiếng còi báo động vang lên trong đầu tôi. Vì tôi biết có một số quy tắc rất cụ thể về cách các giá trị chuỗi ép kiểu thành số, tôi nhận ra rằng thao tác này **không** an toàn về kiểu. Vì vậy, thay vào đó, tôi tiếp cận nó theo cách khác:

```js
// lấy ra số phiên bản từ API URL
versionDigit = API_BASE_URL.match(/\/api\/(\d+)$/)[1];

// đảm bảo phiên bản thực sự là một số
APIVersion = Number(versionDigit);
```

Tôi biết rằng `API_BASE_URL` là một chuỗi, và tôi còn biết thêm định dạng nội dung của nó bao gồm `".../api/{digits}"` ở cuối. Điều đó cho tôi biết rằng kết quả khớp biểu thức chính quy sẽ thành công, vì vậy việc truy cập mảng `[1]` là an toàn về kiểu.

Tôi cũng biết rằng `versionDigit` sẽ giữ một chuỗi, bởi vì đó là những gì các kết quả khớp biểu thức chính quy trả về. Bây giờ, tôi biết an toàn khi ép kiểu chuỗi chữ số đó thành một số với `Number(..)`.

Theo định nghĩa của tôi, kiểu suy nghĩ đó, và kiểu viết code đó, là có nhận thức về kiểu. Nhận thức về kiểu trong lập trình có nghĩa là suy nghĩ cẩn thận về việc liệu những thứ như vậy có *rõ ràng* và *dễ hiểu* đối với người đọc code hay không.

### Nhận thức về kiểu *với* TypeScript

Những người hâm mộ TypeScript sẽ chỉ ra rằng TypeScript có thể, thông qua suy luận kiểu (type inference), thực hiện định kiểu tĩnh (thực thi) mà không cần một chú thích kiểu nào trong chương trình. Vì vậy, tất cả các ví dụ code tôi đã chia sẻ trong phần trước, TypeScript cũng có thể xử lý và cung cấp hương vị thực thi kiểu tĩnh thời gian biên dịch của nó.

Nói cách khác, TypeScript sẽ cung cấp cho chúng ta cùng một loại lợi ích trong kiểm tra kiểu, bất kể chúng ta viết cái nào trong hai cái này:

```ts
let API_BASE_URL: string = "https://some.tld/api/2";

// so với:

let API_BASE_URL = "https://some.tld/api/2";
```

Nhưng không có bữa trưa nào miễn phí. Chúng ta có một số vấn đề cần giải quyết. Trước hết, TypeScript *không* kích hoạt một lỗi ở đây:

```js
API_BASE_URL = "https://some.tld/api/2";

APIVersion = Number(API_BASE_URL);
// NaN
```

Theo trực giác, *tôi* muốn một hệ thống nhận thức về kiểu hiểu tại sao điều đó không an toàn. Nhưng có lẽ điều đó là đòi hỏi quá nhiều. Hoặc có lẽ nếu chúng ta thực sự xác định một kiểu hẹp/cụ thể hơn cho biến `API_BASE_URL` đó, thay vì chỉ đơn giản là `string`, nó có thể giúp ích? Chúng ta có thể sử dụng một thủ thuật TypeScript được gọi là "Template Literal Types": [^TSLiteralTypes]

```ts
type VersionedURL = `https://some.tld/api/${number}`;

API_BASE_URL: VersionedURL = "https://some.tld/api/2";

APIVersion = Number(API_BASE_URL);
// NaN
```

Không, TypeScript vẫn không thấy bất kỳ vấn đề nào với điều đó. Vâng, tôi biết có một lời giải thích cho lý do tại sao (cách bản thân `Number(..)` được định kiểu).

| NOTE: |
| :--- |
| Tôi tưởng tượng những người thực sự thông minh *biết* rõ TypeScript có những ý tưởng sáng tạo về cách chúng ta có thể uốn éo bản thân để tạo ra một lỗi ở đó. Có lẽ thậm chí có cả tá cách khác nhau để buộc TypeScript kích hoạt trên code đó. Nhưng đó không thực sự là quan điểm. |

Quan điểm của tôi là, chúng ta không thể hoàn toàn dựa vào các kiểu TypeScript để giải quyết tất cả các vấn đề của mình, cho phép chúng ta kiểm tra và vẫn hoàn toàn không biết gì về các sắc thái của các kiểu và, trong trường hợp này, các hành vi ép kiểu.

Nhưng! Bạn chắc chắn đang phản đối dòng lập luận này, tuyệt vọng để khẳng định rằng ngay cả khi TypeScript không thể hiểu một tình huống cụ thể nào đó, chắc chắn việc sử dụng TypeScript không làm cho nó *tồi tệ hơn*! Đúng không!?

Hãy xem TypeScript nói gì[^TSExample1] về dòng này:

```ts
type VersionedURL = `https://some.tld/api/${number}`;

let API_BASE_URL: VersionedURL = "https://some.tld/api/2";

let versionDigit = API_BASE_URL.match(/\/api\/(\d+)$/)[1];
// Object is possibly 'null'.
```

Lỗi chỉ ra rằng quyền truy cập `[1]` không an toàn về kiểu, bởi vì nếu biểu thức chính quy không tìm thấy bất kỳ kết quả khớp nào trên chuỗi, `match(..)` trả về `null`.

Bạn thấy đấy, mặc dù *tôi* có thể suy luận về nội dung của chuỗi so với cách biểu thức chính quy được viết, và ngay cả khi *tôi* đã cất công để làm cho TypeScript cực kỳ rõ ràng chính xác những nội dung chuỗi cụ thể đó là gì, nó vẫn không đủ thông minh để sắp xếp hai thứ đó lại với nhau để thấy rằng thực sự hoàn toàn an toàn về kiểu khi cho rằng kết quả khớp xảy ra.

| TIP: |
| :--- |
| Có thực sự là công việc của, và cách sử dụng tốt nhất của, một công cụ nhận thức về kiểu để bị uốn éo để diễn đạt mọi sắc thái có thể có của an toàn kiểu không? Chúng ta không cần các công cụ hoàn hảo và phổ quát để thu được vô số lợi ích từ những thứ chúng *có thể* làm. |

Hơn nữa, so sánh phong cách code trong phần trước với code trong phần này (có hoặc không có chú thích), liệu TypeScript có thực sự làm cho việc viết code của chúng ta nhận thức về kiểu hơn không?

Giống như, những thứ `type VersionedURL = ..` và `API_BASE_URL: VersionedURL` đó có *thực sự* làm cho code của chúng ta nhận thức về kiểu rõ ràng hơn không? Tôi không nhất thiết nghĩ như vậy.

### Trí thông minh của TypeScript

Vâng, tôi nghe thấy bạn đang hét vào mặt tôi qua màn hình máy tính. Vâng, tôi biết rằng TypeScript cung cấp những thông tin kiểu mà nó khám phá (hoặc suy luận) cho trình soạn thảo code của bạn, thông qua các hình thức tự động hoàn thành thông minh, các dấu hiệu cảnh báo nội tuyến hữu ích, v.v.

Nhưng tôi đang lập luận rằng ngay cả *chúng* cũng không, tự bản thân chúng, làm cho bạn nhận thức về kiểu hơn với tư cách là một nhà phát triển

Tại sao? Bởi vì nhận thức về kiểu *không* chỉ là về trải nghiệm viết code. Nó cũng là về trải nghiệm đọc, có lẽ thậm chí còn hơn thế nữa. Và không phải tất cả các nơi/cơ chế mà code được đọc, đều có quyền truy cập để hưởng lợi từ tất cả trí thông minh bổ sung đó.

Nhìn xem, sự kỳ diệu của một language-server bơm trí thông minh vào trình soạn thảo code của bạn là không thể nghi ngờ là tuyệt vời. Nó rất tuyệt và siêu hữu ích.

Và tôi không ghen tị với TypeScript như một công cụ suy luận những thứ về **code JS** của tôi và đưa ra cho tôi các gợi ý và đề xuất thông qua các tích hợp trình soạn thảo code thú vị. Tôi chỉ không nhất thiết muốn *phải* chú thích thông tin kiểu theo một cách cực kỳ cụ thể nào đó chỉ để làm im lặng những lời phàn nàn của công cụ.

### Tiêu chuẩn cao hơn TypeScript

Nhưng ngay cả khi tôi đã/có tất cả những thứ đó, nó vẫn chưa ***đủ*** để tôi hoàn toàn nhận thức về kiểu, cả với tư cách là người viết code và người đọc code.

Những công cụ này không bắt được mọi lỗi kiểu có thể xảy ra, bất kể chúng ta muốn tự nhủ rằng chúng có thể làm được bao nhiêu, và bất kể bao nhiêu vòng lặp và sự uốn éo mà chúng ta chịu đựng để mong muốn điều đó. Tất cả những nỗ lực để dụ dỗ và *ép buộc* một công cụ bắt những lỗi sắc thái đó, thông qua sự phức tạp ngày càng tăng của các thủ thuật cú pháp kiểu, là... tốt nhất là, nỗ lực đặt sai chỗ.

Hơn nữa, không có công cụ nào miễn nhiễm với các lỗi dương tính giả (false positives), phàn nàn về những thứ thực sự không phải là lỗi; những công cụ này sẽ không bao giờ thông minh như con người chúng ta. Bạn thực sự đang lãng phí thời gian của mình để theo đuổi một số thủ thuật cú pháp kỳ quặc để làm dịu những lời phàn nàn của công cụ.

Đơn giản là không có sự thay thế nào, nếu bạn muốn thực sự trở thành một người viết code và người đọc code có nhận thức về kiểu, từ việc học cách các hệ thống kiểu tích hợp sẵn của ngôn ngữ hoạt động. Và vâng, điều đó có nghĩa là mọi nhà phát triển trong nhóm của bạn cần phải nỗ lực để học nó. Bạn không thể làm loãng thứ này chỉ để dễ đạt được hơn cho các nhà phát triển ít kinh nghiệm hơn trong dự án/nhóm.

Ngay cả khi chúng tôi chấp nhận rằng bạn có thể tránh 100% tất cả các ép kiểu *ngầm định* -- bạn không thể -- bạn hoàn toàn sẽ phải đối mặt với nhu cầu *ép kiểu* tường minh -- tất cả các chương trình đều làm vậy!

Và nếu câu trả lời của bạn cho thực tế đó là đề nghị rằng bạn sẽ chỉ trút bỏ gánh nặng tinh thần của việc hiểu chúng cho một công cụ như TypeScript... thì tôi xin lỗi phải nói với bạn, nhưng bạn đang thiếu hụt một cách rõ ràng và đau đớn so với tiêu chuẩn *nhận thức về kiểu* mà tôi đang thách thức tất cả các nhà phát triển phấn đấu hướng tới.

Tôi không ủng hộ, ở đây, việc bạn từ bỏ TypeScript. Nếu bạn thích nó, tốt thôi. Nhưng tôi đang rất rõ ràng và nhiệt tình thách thức bạn: hãy ngừng sử dụng TypeScript như một cái nạng. Hãy ngừng quỳ gối để xoa dịu các chúa tể engine TypeScript. Hãy ngừng theo đuổi một cách ngu ngốc mọi con thỏ kiểu xuống mọi cái hang cú pháp.

Từ quan sát của tôi, có một mối quan hệ nghịch đảo bi thảm giữa việc sử dụng các công cụ nhận thức về kiểu (như TypeScript) và mong muốn/nỗ lực theo đuổi nhận thức về kiểu thực tế với tư cách là người viết code và người đọc code. Bạn càng dựa vào TypeScript, dường như bạn càng bị cám dỗ và khuyến khích chuyển sự chú ý của mình ra khỏi hệ thống kiểu của JS (và đặc biệt là khỏi ép kiểu) sang hệ thống kiểu thay thế của TypeScript.

Thật không may, TypeScript không bao giờ có thể thoát hoàn toàn khỏi hệ thống kiểu của JS, bởi vì các kiểu của TypeScript bị trình biên dịch *xóa bỏ*, và những gì còn lại chỉ là JS mà engine JS phải đương đầu.

| TIP: |
| :--- |
| Hãy tưởng tượng nếu ai đó đưa cho bạn một cốc nước lọc để uống. Và ngay trước khi bạn nhấp một ngụm, họ nói, "Chúng tôi đã chiết xuất nước đó từ lòng đất gần một bãi rác thải. Nhưng đừng lo, chúng tôi đã sử dụng một bộ lọc hoàn toàn tuyệt vời, và nước đó hoàn toàn an toàn!" Bạn tin tưởng bộ lọc đó bao nhiêu? Hơn nữa đối với quan điểm chung của tôi, bạn có cảm thấy thoải mái hơn khi uống nước đó nếu bạn hiểu mọi thứ về nguồn nước, tất cả các quy trình lọc, và mọi thứ có *trong* nước của chiếc cốc trên tay bạn không!? Hay là tin tưởng bộ lọc đó là đủ tốt? |

### So sánh bằng có nhận thức về kiểu (Type Aware Equality)

Tôi sẽ kết thúc chương dài ngoằng này bằng một minh họa cuối cùng, mô hình hóa cách tôi nghĩ các nhà phát triển nên -- được trang bị tư duy phản biện thay vì chạy theo số đông -- tiếp cận việc viết code có nhận thức về kiểu, cho dù bạn có sử dụng công cụ như TypeScript hay không.

Chúng ta sẽ lại xem xét các phép so sánh bằng (`==` vs `===`), từ góc độ nhận thức về kiểu. Trước đó trong chương này, tôi đã hứa rằng tôi sẽ đưa ra lập luận ủng hộ `==` hơn là `===`, vì vậy đây:

Hãy nhắc lại/tóm tắt những gì chúng ta biết về `==` và `===` cho đến nay:

1. Nếu kiểu của các toán hạng cho `==` khớp nhau, nó hoạt động *giống hệt* như `===`.

2. Nếu kiểu của các toán hạng cho `===` không khớp nhau, nó sẽ luôn trả về `false`.

3. Nếu kiểu của các toán hạng cho `==` không khớp nhau, nó sẽ cho phép ép kiểu một trong hai toán hạng (thường ưu tiên giá trị kiểu số), cho đến khi các kiểu cuối cùng khớp nhau; một khi chúng khớp nhau, xem (1).

OK, vì vậy hãy lấy những thực tế đó và phân tích cách chúng có thể tương tác trong chương trình của chúng ta.

Nếu bạn đang thực hiện so sánh bằng giữa `x` và `y` như thế này:

```js
if ( /* x và y có bằng nhau không */ ) {
    // ..
}
```

Các điều kiện có thể xảy ra mà chúng ta có thể gặp phải, liên quan đến kiểu của `x` và `y` là gì?

1. Chúng ta có thể biết chính xác (các) kiểu mà `x` và `y` có thể là, bởi vì chúng ta biết cách các biến đó được gán.

2. Hoặc chúng ta có thể không biết những kiểu đó có thể là gì. Có thể là `x` hoặc `y` có thể là bất kỳ kiểu nào, hoặc ít nhất là bất kỳ kiểu nào trong số nhiều kiểu khác nhau, sao cho các tổ hợp kiểu có thể có trong phép so sánh quá phức tạp để hiểu/dự đoán.

Chúng ta có thể đồng ý rằng (1) thích hợp hơn nhiều so với (2) không? Chúng ta có thể đồng ý thêm rằng (1) đại diện cho việc đã viết code của chúng ta theo cách có nhận thức về kiểu, trong khi (2) đại diện cho code hoàn toàn *không nhận thức* về kiểu?

Nếu bạn đang sử dụng TypeScript, bạn rất có thể biết về kiểu của `x` và `y`, đúng không? Ngay cả khi bạn không sử dụng TypeScript, chúng tôi đã chỉ ra rằng bạn có thể thực hiện các bước có chủ đích để viết code theo cách mà kiểu của `x` và `y` được biết đến và rõ ràng.

#### (2) Kiểu không xác định

Nếu bạn đang ở trong tình huống (2), tôi sẽ khẳng định rằng code của bạn đang ở trạng thái có vấn đề. Code của bạn chưa tối ưu. Code của bạn cần được tái cấu trúc (refactor). Điều tốt nhất nên làm, nếu bạn tìm thấy code trong trạng thái này, là... sửa nó!

Thay đổi code để nó có nhận thức về kiểu. Nếu điều đó có nghĩa là sử dụng TypeScript, và thậm chí chèn một số chú thích kiểu, hãy làm điều đó. Hoặc nếu bạn cảm thấy bạn có thể đạt được trạng thái nhận thức về kiểu chỉ với *JS thuần*, hãy làm điều đó. Dù bằng cách nào, hãy làm bất cứ điều gì bạn có thể để đạt được kịch bản (1).

Nếu bạn không thể đảm bảo code thực hiện so sánh bằng giữa `x` và `y` là có nhận thức về kiểu, và bạn không có lựa chọn nào khác, thì bạn hoàn toàn *phải* sử dụng toán tử so sánh bằng nghiêm ngặt `===`. Không làm như vậy sẽ là cực kỳ vô trách nhiệm.

```js
if (x === y) {
    // ..
}
```

Nếu bạn không biết gì về các kiểu, làm sao bạn (hoặc bất kỳ người đọc nào khác trong tương lai của code của bạn) có bất kỳ ý tưởng nào về cách các bước ép kiểu trong `==` sẽ hoạt động!? Bạn không thể.

Điều duy nhất có trách nhiệm cần làm là, tránh ép kiểu và sử dụng `===`.

Nhưng đừng quên mất thực tế này: bạn chỉ đang chọn `===` như là phương sách cuối cùng, khi code của bạn quá thiếu nhận thức về kiểu -- e hèm, hỏng kiểu! -- đến mức không có lựa chọn nào khác.

#### (1) Kiểu đã biết

OK, thay vào đó hãy giả sử bạn đang ở trong kịch bản (1). Bạn biết kiểu của `x` và `y`. Rất rõ ràng trong code tập hợp hẹp các kiểu tham gia vào kiểm tra so sánh bằng này có thể là gì.

Tuyệt vời!

Nhưng vẫn có hai điều kiện phụ có thể xảy ra mà bạn có thể gặp phải:

* (1a): `x` và `y` có thể đã cùng kiểu, cho dù cả hai đều là `string`, `number`, v.v.

* (1b): `x` và `y` có thể khác kiểu.

Hãy xem xét từng trường hợp này riêng lẻ.

##### (1a) Các kiểu khớp đã biết

Nếu các kiểu trong so sánh bằng khớp nhau (bất kể chúng là gì), chúng ta đã biết chắc chắn rằng `==` và `===` thực hiện chính xác điều tương tự. Hoàn toàn không có sự khác biệt.

Ngoại trừ, `==` *ngắn hơn* một ký tự. Hầu hết các nhà phát triển cảm thấy theo bản năng rằng phiên bản ngắn gọn nhất nhưng tương đương của một cái gì đó thường thích hợp hơn. Điều đó không phải là phổ quát, tất nhiên, nhưng ít nhất đó là một sở thích chung.

```js
// đây là tốt nhất
if (x == y) {
    // ..
}
```

Trong trường hợp cụ thể này, thêm một dấu `=` sẽ không làm gì cho chúng ta để khiến code rõ ràng hơn. Trên thực tế, nó thực sự sẽ làm cho phép so sánh tồi tệ hơn!

```js
// điều này hoàn toàn tồi tệ hơn ở đây!
if (x === y) {
    // ..
}
```

Tại sao nó tồi tệ hơn?

Bởi vì trong kịch bản (2), chúng ta đã xác định rằng `===` được sử dụng cho phương sách cuối cùng khi chúng ta không biết đủ/bất cứ điều gì về các kiểu để có thể dự đoán kết quả. Chúng ta sử dụng `===` khi chúng ta muốn đảm bảo rằng chúng ta đang tránh ép kiểu khi chúng ta biết ép kiểu có thể xảy ra.

Nhưng điều đó không áp dụng ở đây! Chúng ta đã biết rằng không có ép kiểu nào xảy ra. Không có lý do gì để gây nhầm lẫn cho người đọc với một `===` ở đây. Nếu bạn sử dụng `===` ở một nơi mà bạn đã *biết* các kiểu -- và hơn nữa, chúng khớp nhau! -- điều đó thực sự có thể gửi một tín hiệu hỗn hợp cho người đọc. Họ có thể đã giả định rằng họ biết điều gì sẽ xảy ra trong kiểm tra so sánh bằng, nhưng sau đó họ thấy `===` và họ nghi ngờ chính mình!

Một lần nữa, để nói một cách rõ ràng, nếu bạn biết kiểu của một so sánh bằng, và bạn biết chúng khớp nhau, chỉ có một lựa chọn đúng: `==`.

```js
// hãy gắn bó với tùy chọn này
if (x == y) {
    // ..
}
```

##### (1b) Các kiểu không khớp đã biết

OK, chúng ta đang ở trong kịch bản cuối cùng. Chúng ta cần so sánh `x` và `y`, và chúng ta biết kiểu của chúng, nhưng chúng ta cũng biết kiểu của chúng **KHÔNG** giống nhau.

Chúng ta nên sử dụng toán tử nào ở đây?

Nếu bạn chọn `===`, bạn đã phạm một sai lầm lớn. Tại sao!? Bởi vì `===` được sử dụng với các kiểu không khớp đã biết sẽ không bao giờ, không bao giờ, không bao giờ trả về `true`. Nó sẽ luôn thất bại.

```js
// `x` và `y` có kiểu khác nhau?
if (x === y) {
    // chúc mừng, code trong này sẽ KHÔNG BAO GIỜ chạy
}
```

OK. Vì vậy, `===` bị loại khi các kiểu đã biết và không khớp. Lựa chọn duy nhất khác của chúng ta là gì?

Vâng, thực ra, chúng ta lại có hai lựa chọn. Chúng ta *có thể* quyết định:

* (1b-1): Hãy thay đổi code để chúng ta không cố gắng thực hiện kiểm tra so sánh bằng với các kiểu không khớp đã biết; điều đó có thể liên quan đến việc ép kiểu rõ ràng một hoặc cả hai giá trị để các kiểu bây giờ khớp nhau, trong trường hợp đó quay trở lại kịch bản (1a).

* (1b-2): Nếu chúng ta sẽ so sánh các kiểu không khớp đã biết để kiểm tra bằng nhau, và chúng ta muốn có bất kỳ hy vọng nào về việc kiểm tra đó bao giờ cũng vượt qua, chúng ta *phải* sử dụng `==`, bởi vì nó là toán tử duy nhất trong số các toán tử so sánh bằng có thể ép kiểu một hoặc cả hai toán hạng cho đến khi các kiểu khớp nhau.

```js
// `x` và `y` có kiểu khác nhau,
// vì vậy hãy cho phép JS ép kiểu chúng
// để so sánh bằng
if (x == y) {
    // .. (vậy, bạn đang nói là có cơ hội?)
}
```

Đó là tất cả. Chúng ta đã xong. Chúng ta đã xem xét mọi điều kiện so sánh bằng nhạy cảm với kiểu (giữa `x` và `y`).

#### Tóm tắt so sánh bằng nhạy cảm với kiểu

Trường hợp luôn ưu tiên `==` hơn `===` như sau:

1. Cho dù bạn có sử dụng TypeScript hay không -- nhưng đặc biệt nếu bạn *có* sử dụng TypeScript -- mục tiêu phải là có mọi phần của code, bao gồm tất cả các so sánh bằng, là *có nhận thức về kiểu*.

2. Nếu bạn biết các kiểu, bạn phải luôn ưu tiên `==`.

    - Trong trường hợp các kiểu khớp nhau, `==` vừa ngắn hơn vừa đúng đắn hơn cho việc kiểm tra.

    - Trong trường hợp các kiểu không khớp, `==` là toán tử duy nhất có thể ép kiểu (các) toán hạng cho đến khi các kiểu khớp nhau, vì vậy đó là cách duy nhất mà một kiểm tra như vậy có thể hy vọng vượt qua.

3. Cuối cùng, chỉ khi bạn *không thể* biết/dự đoán các kiểu, vì một lý do khó chịu nào đó, và bạn không có lựa chọn nào khác, hãy quay lại sử dụng `===` như một phương sách cuối cùng. Và có lẽ thêm một bình luận code ở đó thừa nhận lý do tại sao `===` được sử dụng, và có thể nhắc nhở một nhà phát triển tương lai sau này thay đổi code để sửa chữa khiếm khuyết đó và loại bỏ cái nạng `===`.

#### Vấn đề không nhất quán của TypeScript

Hãy để tôi nói cực kỳ rõ ràng: nếu bạn đang sử dụng TypeScript đúng cách, và bạn biết các kiểu của một so sánh bằng, việc sử dụng `===` cho so sánh đó là hoàn toàn *sai*! Chấm hết.

Vấn đề là, TypeScript một cách kỳ lạ và gây nản lòng vẫn yêu cầu bạn sử dụng `===`, trừ khi nó đã biết rằng các kiểu đã khớp.

Đó là bởi vì TypeScript hoặc không hiểu đầy đủ về nhận thức về kiểu và ép kiểu, hoặc -- và điều này thậm chí còn gây phẫn nộ hơn! -- nó hiểu đầy đủ nhưng nó vẫn coi thường hệ thống kiểu của JS đến mức tránh xa ngay cả những lý luận cơ bản nhất về nhận thức kiểu.

Không tin tôi sao? Nghĩ rằng tôi quá khắt khe? Hãy thử cái này trong TypeScript: [^TSExample2]

```js
let result = (42 == "42");
// Điều kiện này sẽ luôn trả về 'false' vì
// các kiểu 'number' và 'string' không có sự trùng lặp nào.
```

Tôi cạn lời để mô tả điều đó làm tôi khó chịu đến mức nào. Nếu bạn đã chú ý đến chương dài, nặng nề này, bạn biết rằng TypeScript về cơ bản đang nói dối ở đây. Tất nhiên `42 == "42"` sẽ tạo ra `true` trong JS.

Chà, nó không phải là một lời nói dối, nhưng nó đang phơi bày một sự thật cơ bản mà rất nhiều người vẫn chưa đánh giá cao đầy đủ: TypeScript hoàn toàn vứt bỏ các quy tắc bình thường của hệ thống kiểu của JS, bởi vì quan điểm của TypeScript là hệ thống kiểu của JS -- và đặc biệt là ép kiểu ngầm định -- là xấu, và cần phải được thay thế.

Trong thế giới của TypeScript, `42` và `"42"` không bao giờ có thể bằng nhau. Do đó thông báo lỗi. Nhưng trong vùng đất JS, `42` và `"42"` hoàn toàn bằng nhau về mặt ép kiểu. Và tôi tin rằng tôi đã đưa ra một trường hợp mạnh mẽ ở đây rằng chúng *nên được* giả định là tương đương về mặt ép kiểu một cách an toàn.

Điều làm tôi bận tâm hơn nữa là, TypeScript có nhiều sự không nhất quán về khía cạnh này. TypeScript hoàn toàn ổn với việc ép kiểu *ngầm định* trong code này:

```js
irony = `The value '42' and ${42} are coercively equal.`;
```

`42` được ép kiểu ngầm định thành một chuỗi khi nội suy nó vào câu. Tại sao TypeScript ổn với việc ép kiểu ngầm định này, nhưng không phải là ép kiểu ngầm định `42 == "42"`?

TypeScript cũng không phàn nàn về code này:

```js
API_BASE_URL = "https://some.tld/api/2";
if (API_BASE_URL) {
    // ..
}
```

Tại sao `ToBoolean()` là một ép kiểu ngầm định OK, nhưng `ToNumber()` trong thuật toán `==` thì không?

Tôi sẽ để bạn suy ngẫm về điều này: bạn có thực sự nghĩ rằng đó là một ý tưởng hay khi viết code cuối cùng sẽ chạy trong engine JS, nhưng sử dụng một công cụ và phong cách code đã cố ý loại bỏ hầu hết toàn bộ một trụ cột của ngôn ngữ JS? Hơn nữa, liệu có ổn không khi nó cũng bị đảo lộn với nhiều ngoại lệ không nhất quán, chỉ đơn giản là để phục vụ cho những thói quen cũ của các nhà phát triển JS?

## Còn gì nữa?

Tôi hy vọng đến bây giờ bạn đã cảm thấy hiểu rõ hơn nhiều về cách hệ thống kiểu của JS hoạt động, từ các kiểu giá trị nguyên thủy đến các kiểu đối tượng, cho đến cách các ép kiểu được thực hiện bởi engine.

Quan trọng hơn, giờ đây bạn cũng có một bức tranh hoàn chỉnh hơn nhiều về những ưu/nhược điểm của các lựa chọn mà chúng ta đưa ra khi sử dụng hệ thống kiểu của JS, chẳng hạn như chọn ép kiểu *ngầm định* hay *tường minh* ở các điểm khác nhau.

Nhưng chúng ta vẫn chưa bao quát hết bối cảnh mà hệ thống kiểu hoạt động. Trong phần còn lại của cuốn sách này, chúng ta sẽ chuyển sự chú ý sang các quy tắc cú pháp/ngữ pháp của JS chi phối cách các toán tử và câu lệnh hoạt động.

[^EichCoercion]: "The State of JavaScript - Brendan Eich", luồng bình luận, Hacker News; 9 tháng 10 năm 2012; https://news.ycombinator.com/item?id=4632704 ; Truy cập tháng 8 năm 2022

[^CrockfordCoercion]: "JavaScript: The World's Most Misunderstood Programming Language"; 2001; https://www.crockford.com/javascript/javascript.html ; Truy cập tháng 8 năm 2022

[^CrockfordIfs]: "json2.js", Github; 21 tháng 4 năm 2018; https://github.com/douglascrockford/JSON-js/blob/8e8b0407e475e35942f7e9461dab81929fcc7321/json2.js#L336 ; Truy cập tháng 8 năm 2022

[^BrendanToString]: danh sách gửi thư ESDiscuss; 26 tháng 8 năm 2014; https://esdiscuss.org/topic/string-symbol#content-15 ; Truy cập tháng 8 năm 2022

[^AbstractOperations]: "7.1 Type Conversion", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-type-conversion ; Truy cập tháng 8 năm 2022

[^ToBoolean]: "7.1.2 ToBoolean(argument)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-toboolean ; Truy cập tháng 8 năm 2022

[^ExoticFalsyObjects]: "B.3.6 The [[IsHTMLDDA]] Internal Slot", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-IsHTMLDDA-internal-slot ; Truy cập tháng 8 năm 2022

[^OrdinaryToPrimitive]: "7.1.1.1 OrdinaryToPrimitive(O,hint)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-ordinarytoprimitive ; Truy cập tháng 8 năm 2022

[^ToString]: "7.1.17 ToString(argument)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-tostring ; Truy cập tháng 8 năm 2022

[^StringConstructor]: "22.1.1 The String Constructor", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-string-constructor ; Truy cập tháng 8 năm 2022

[^StringFunction]: "22.1.1.1 String(value)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-string-constructor-string-value ; Truy cập tháng 8 năm 2022

[^ToNumber]: "7.1.4 ToNumber(argument)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-tonumber ; Truy cập tháng 8 năm 2022

[^ToNumeric]: "7.1.3 ToNumeric(argument)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-tonumeric ; Truy cập tháng 8 năm 2022

[^NumberConstructor]: "21.1.1 The Number Constructor", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-number-constructor ; Truy cập tháng 8 năm 2022

[^NumberFunction]: "21.1.1.1 Number(value)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-number-constructor-number-value ; Truy cập tháng 8 năm 2022

[^SameValue]: "7.2.11 SameValue(x,y)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-samevalue ; Truy cập tháng 8 năm 2022

[^StrictEquality]: "7.2.16 IsStrictlyEqual(x,y)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-isstrictlyequal ; Truy cập tháng 8 năm 2022

[^LooseEquality]: "7.2.15 IsLooselyEqual(x,y)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-islooselyequal ; Truy cập tháng 8 năm 2022

[^NumericAbstractOps]: "6.1.6 Numeric Types", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-numeric-types ; Truy cập tháng 8 năm 2022

[^NumberEqual]: "6.1.6.1.13 Number:equal(x,y)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-numeric-types-number-equal ; Truy cập tháng 8 năm 2022

[^BigIntEqual]: "6.1.6.2.13 BigInt:equal(x,y)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-numeric-types-bigint-equal ; Truy cập tháng 8 năm 2022

[^LessThan]: "7.2.14 IsLessThan(x,y,LeftFirst)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-islessthan ; Truy cập tháng 8 năm 2022

[^StringPrefix]: "7.2.9 IsStringPrefix(p,q)", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-isstringprefix ; Truy cập tháng 8 năm 2022

[^SymbolString]: "String(symbol)", danh sách gửi thư ESDiscuss; 12 tháng 8 năm 2014; https://esdiscuss.org/topic/string-symbol ; Truy cập tháng 8 năm 2022

[^ASMjs]: "ASM.js - Working Draft"; 18 tháng 8 năm 2014; http://asmjs.org/spec/latest/ ; Truy cập tháng 8 năm 2022

[^TSExample1]: "TypeScript Playground"; https://tinyurl.com/ydkjs-ts-example-1 ; Truy cập tháng 8 năm 2022

[^TSExample2]: "TypeScript Playground"; https://tinyurl.com/ydkjs-ts-example-2 ; Truy cập tháng 8 năm 2022

[^TSLiteralTypes]: "TypeScript 4.1, Template Literal Types"; https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-1.html#template-literal-types ; Truy cập tháng 8 năm 2022
