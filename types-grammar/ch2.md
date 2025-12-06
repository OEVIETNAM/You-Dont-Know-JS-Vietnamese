# Chương 2: Các Hành Vi Nguyên Thủy

Cho đến nay, chúng ta đã khám phá bảy kiểu giá trị nguyên thủy tích hợp sẵn trong JS: `null`, `undefined`, `boolean`, `string`, `number`, `bigint`, và `symbol`.

Chương 1 đã có khá nhiều thứ để tiếp thu, phức tạp hơn nhiều so với tôi cá là hầu hết độc giả mong đợi. Nếu bạn vẫn đang lấy lại hơi thở sau khi đọc tất cả những điều đó, đừng lo lắng về việc nghỉ ngơi một chút trước khi tiếp tục ở đây!

Khi bạn đã tỉnh táo và sẵn sàng tiếp tục, hãy cùng tìm hiểu sâu hơn về các hành vi nhất định được ngụ ý bởi các kiểu giá trị đối với tất cả các giá trị tương ứng của chúng. Chúng ta sẽ xem xét cẩn thận và kỹ lưỡng hơn về tất cả các hành vi khác nhau này.

## Tính Bất Biến Của Nguyên Thủy (Primitive Immutability)

Tất cả các giá trị nguyên thủy đều là bất biến (immutable), có nghĩa là không có gì trong chương trình JS có thể can thiệp vào nội dung của giá trị và sửa đổi nó theo bất kỳ cách nào.

```js
myAge = 42;

// later:

myAge = 43;
```

Câu lệnh `myAge = 43` không thay đổi giá trị. Nó gán lại một giá trị khác `43` cho `myAge`, thay thế hoàn toàn giá trị trước đó là `42`.

Các giá trị mới cũng được tạo ra thông qua các hoạt động khác nhau, nhưng một lần nữa những điều này không sửa đổi giá trị ban đầu:

```js
42 + 1;             // 43

"Hello" + "!";      // "Hello!"
```

Các giá trị `43` và `"Hello!"` là mới, các giá trị riêng biệt so với các giá trị `42` và `"Hello"` trước đó, tương ứng.

Ngay cả một giá trị chuỗi, trông giống như chỉ là một mảng các ký tự -- và nội dung mảng thường có thể thay đổi được (mutable) -- vẫn là bất biến:

```js
greeting = "Hello.";

greeting[5] = "!";

console.log(greeting);      // Hello.
```

| CẢNH BÁO: |
| :--- |
| Trong chế độ không nghiêm ngặt (non-strict mode), việc gán cho một thuộc tính chỉ đọc (như `greeting[5] = ..`) sẽ thất bại trong im lặng. Trong chế độ nghiêm ngặt (strict-mode), việc gán không được phép sẽ ném ra một ngoại lệ. |

Bản chất của các giá trị nguyên thủy là bất biến không bị ảnh hưởng *theo bất kỳ cách nào* bởi cách biến hoặc thuộc tính đối tượng giữ giá trị được khai báo. Ví dụ, cho dù `const`, `let`, hay `var` được sử dụng để khai báo biến `greeting` ở trên, giá trị chuỗi mà nó giữ là bất biến.

`const` không tạo ra các giá trị bất biến, nó khai báo các biến không thể được gán lại (hay còn gọi là gán bất biến) -- xem tiêu đề "Phạm Vi & Closures" của bộ sách này để biết thêm thông tin.

Một thuộc tính trên một đối tượng có thể được đánh dấu là chỉ đọc -- với thuộc tính mô tả `writable: false`, như đã thảo luận trong tiêu đề "Đối Tượng & Các Lớp" của bộ sách này. Nhưng điều đó vẫn không ảnh hưởng đến bản chất của giá trị, chỉ ảnh hưởng đến việc ngăn chặn việc gán lại thuộc tính.

### Các Giá Trị Nguyên Thủy Có Thuộc Tính?

Ngoài ra, các thuộc tính *không thể* được thêm vào bất kỳ giá trị nguyên thủy nào:

```js
greeting = "Hello.";

greeting.isRendered = true;

greeting.isRendered;        // undefined
```

Đoạn mã này trông giống như nó đang thêm một thuộc tính `isRendered` vào giá trị trong `greeting`, nhưng phép gán này thất bại trong im lặng (ngay cả trong chế độ nghiêm ngặt).

Truy cập thuộc tính không được phép theo bất kỳ cách nào trên các giá trị nguyên thủy nullish là `null` và `undefined`. Nhưng các thuộc tính *có thể* được truy cập trên tất cả các giá trị nguyên thủy khác -- vâng, điều đó nghe có vẻ phản trực giác.

Ví dụ, tất cả các giá trị chuỗi đều có thuộc tính `length` chỉ đọc:

```js
greeting = "Hello.";

greeting.length;            // 6
```

`length` không thể được đặt, nhưng nó có thể được truy cập, và nó hiển thị số lượng đơn vị mã (code-units) được lưu trữ trong giá trị (xem "Mã Hóa Ký Tự JS" trong Chương 1), thường có nghĩa là số lượng ký tự trong chuỗi.

| LƯU Ý: |
| :--- |
| Đại loại vậy. Đối với hầu hết các ký tự tiêu chuẩn, điều đó đúng; một ký tự là một điểm mã (code-point), là một đơn vị mã. Tuy nhiên, như đã giải thích trong Chương 1, các ký tự Unicode mở rộng trên điểm mã `65535` sẽ được lưu trữ dưới dạng hai đơn vị mã (cặp thay thế - surrogate halves). Do đó, đối với mỗi ký tự như vậy, `length` sẽ bao gồm `2` trong số đếm của nó, mặc dù ký tự được in trực quan dưới dạng một ký hiệu. |

Các giá trị nguyên thủy không phải nullish cũng có một vài phương thức tích hợp tiêu chuẩn có thể được truy cập:

```js
greeting = "Hello.";

greeting.toString();    // "Hello." <-- dư thừa
greeting.valueOf();     // "Hello."
```

Ngoài ra, hầu hết các kiểu giá trị nguyên thủy định nghĩa các phương thức riêng của chúng với các hành vi cụ thể vốn có cho kiểu đó. Chúng ta sẽ đề cập đến những điều này sau trong chương này.

| LƯU Ý: |
| :--- |
| Như đã đề cập ngắn gọn trong Chương 1, về mặt kỹ thuật, các loại truy cập thuộc tính/phương thức này trên các giá trị nguyên thủy được tạo điều kiện bởi một hành vi ép buộc ngầm định được gọi là *tự động đóng hộp* (auto-boxing). Chúng ta sẽ đề cập chi tiết về điều này trong "Các Đối Tượng Tự Động" trong Chương 3. |

## Các Phép Gán Nguyên Thủy

Bất kỳ phép gán nào của một giá trị nguyên thủy từ biến/thùng chứa này sang biến/thùng chứa khác đều là một *bản sao giá trị* (value-copy):

```js
myAge = 42;

yourAge = myAge;        // được gán bằng bản sao giá trị

myAge;                  // 42
yourAge;                // 42
```

Ở đây, các biến `myAge` và `yourAge` mỗi biến có bản sao riêng của giá trị số `42`.

| LƯU Ý: |
| :--- |
| Bên trong công cụ JS, *có thể* trường hợp là chỉ có một giá trị `42` tồn tại trong bộ nhớ, và công cụ trỏ cả hai biến `myAge` và `yourAge` vào giá trị được chia sẻ. Vì các giá trị nguyên thủy là bất biến, không có nguy hiểm nào khi công cụ JS làm như vậy. Nhưng điều quan trọng đối với chúng ta với tư cách là các nhà phát triển JS là, trong các chương trình của chúng ta, `myAge` và `yourAge` hoạt động như thể chúng có bản sao riêng của giá trị đó, thay vì chia sẻ nó. |

Nếu sau đó chúng ta gán lại `myAge` thành `43` (khi tôi có sinh nhật), nó không ảnh hưởng đến `42` vẫn được gán cho `yourAge`:

```js
myAge++;            // đại loại giống như: myAge = myAge + 1

myAge;              // 43
yourAge;            // 42 <-- không thay đổi
```

## Các Hành Vi Của Chuỗi (String Behaviors)

Các giá trị chuỗi có một số hành vi cụ thể mà mọi nhà phát triển JS nên biết.

### Truy Cập Ký Tự Chuỗi

Mặc dù chuỗi thực sự không phải là mảng, JS cho phép truy cập kiểu mảng `[ .. ]` của một ký tự tại một chỉ số số (dựa trên `0`):

```js
greeting = "Hello!";

greeting[4];            // "o"
```

Nếu giá trị/biểu thức nằm giữa `[ .. ]` không phân giải thành một số, giá trị sẽ được ép buộc ngầm định thành biểu diễn số nguyên/toàn bộ của nó (nếu có thể).

```js
greeting["4"];          // "o"
```

Nếu giá trị/biểu thức phân giải thành một số nằm ngoài phạm vi số nguyên `0` - `length - 1` (hoặc `NaN`), hoặc nếu nó không phải là kiểu giá trị `number`, việc truy cập thay vào đó sẽ được coi là truy cập thuộc tính với tên thuộc tính tương đương chuỗi. Nếu truy cập thuộc tính do đó thất bại, kết quả là `undefined`.

| LƯU Ý: |
| :--- |
|  Chúng ta sẽ đề cập sâu về sự ép buộc (coercion) sau trong cuốn sách. |

### Lặp Ký Tự

Chuỗi không phải là mảng, nhưng chúng chắc chắn bắt chước mảng chặt chẽ theo nhiều cách. Một hành vi như vậy là, giống như mảng, chuỗi có thể lặp lại (iterables). Điều này có nghĩa là các ký tự (đơn vị mã) của một chuỗi có thể được lặp lại riêng lẻ:

```js
myName = "Kyle";

for (let char of myName) {
    console.log(char);
}
// K
// y
// l
// e

chars = [ ...myName ];
chars;
// [ "K", "y", "l", "e" ]
```

Các giá trị, chẳng hạn như chuỗi và mảng, là có thể lặp lại (thông qua `...`, `for..of`, và `Array.from(..)`), nếu chúng hiển thị một phương thức tạo iterator (iterator-producing method) tại vị trí thuộc tính symbol đặc biệt `Symbol.iterator` (xem "Các Symbol Nổi Tiếng" trong Chương 1):

```js
myName = "Kyle";
it = myName[Symbol.iterator]();

it.next();      // { value: "K", done: false }
it.next();      // { value: "y", done: false }
it.next();      // { value: "l", done: false }
it.next();      // { value: "e", done: false }
it.next();      // { value: undefined, done: true }
```

| LƯU Ý: |
| :--- |
| Các chi tiết cụ thể của giao thức iterator, bao gồm thực tế là kết quả `{ value: "e" .. }` vẫn hiển thị `done: false`, được đề cập chi tiết trong tiêu đề "Đồng Bộ & Bất Đồng Bộ" của bộ sách này. |

### Tính Toán Độ Dài (Length Computation)

Như đã đề cập trong Chương 1, các giá trị chuỗi có một thuộc tính `length` tự động hiển thị độ dài của chuỗi; thuộc tính này chỉ có thể được truy cập; các nỗ lực để thiết lập nó bị bỏ qua trong im lặng.

Giá trị `length` được báo cáo phần nào tương ứng với số lượng ký tự trong chuỗi (thực tế là các đơn vị mã), nhưng như chúng ta đã thấy trong Chương 1, nó phức tạp hơn khi các ký tự Unicode có liên quan.

Hầu hết mọi người phân biệt trực quan các biểu tượng là các ký tự riêng biệt; khái niệm về một biểu tượng trực quan độc lập này được gọi là một *grapheme*, hoặc một *cụm grapheme* (grapheme cluster). Vì vậy, khi đếm "độ dài" của một chuỗi, chúng ta thường có nghĩa là chúng ta đang đếm số lượng grapheme.

Nhưng đó không phải là cách máy tính xử lý các ký tự.

Trong JS, mỗi *ký tự* là một đơn vị mã (16 bit), với giá trị điểm mã bằng hoặc thấp hơn `65535`. Thuộc tính `length` của một chuỗi luôn đếm số lượng đơn vị mã trong giá trị chuỗi, không phải điểm mã. Một đơn vị mã có thể đại diện cho một ký tự đơn lẻ, hoặc nó có thể là một phần của một cặp thay thế, hoặc nó có thể được kết hợp với một biểu tượng *kết hợp* liền kề, hoặc một phần của một cụm grapheme. Như vậy, `length` không khớp với khái niệm điển hình về đếm các ký tự/grapheme trực quan.

Để tiến gần hơn đến *độ dài grapheme* mong đợi/trực quan cho một chuỗi, giá trị chuỗi trước tiên cần được chuẩn hóa với `normalize("NFC")` (xem "Chuẩn Hóa Unicode" trong Chương 1) để tạo ra bất kỳ đơn vị mã *composed* nào (nếu có thể), trong trường hợp bất kỳ ký tự nào ban đầu được lưu trữ *decomposed* dưới dạng các đơn vị mã riêng biệt.

Ví dụ:

```js
favoriteItem = "teléfono";
favoriteItem.length;            // 9 -- ôi không!

favoriteItem = favoriteItem.normalize("NFC");
favoriteItem.length;            // 8 -- phù!
```

Thật không may, như chúng ta đã thấy trong Chương 1, chúng ta vẫn sẽ có khả năng các ký tự có điểm mã lớn hơn `65535`, và do đó cần một cặp thay thế để được biểu diễn. Các ký tự như vậy sẽ tính gấp đôi trong `length`:

```js
// "☎" === "\u260E"
oldTelephone = "☎";
oldTelephone.length;            // 1

// "📱" === "\u{1F4F1}" === "\uD83D\uDCF1"
cellphone = "📱";
cellphone.length;               // 2 -- oops!
```

Vậy chúng ta phải làm gì?

Một cách sửa chữa là sử dụng lặp lại ký tự (thông qua toán tử `...`) như chúng ta đã thấy trong phần trước, vì nó tự động trả về từng ký tự kết hợp từ một cặp thay thế:

```js
cellphone = "📱";
cellphone.length;               // 2 -- oops!
[ ...cellphone ].length;        // 1 -- phù!
```

Nhưng, thật không may, các cụm grapheme (như đã giải thích trong Chương 1) lại ném thêm một cái cờ lê vào việc tính toán độ dài của chuỗi. Ví dụ, nếu chúng ta lấy biểu tượng ngón tay cái hướng xuống (`"\u{1F44E}"` và thêm vào đó công cụ sửa đổi tông màu da cho da ngăm đen (`"\u{1F3FE}"`), chúng ta nhận được:

```js
// "👎🏾" = "\u{1F44E}\u{1F3FE}"
thumbsDown = "👎🏾";

thumbsDown.length;              // 4 -- oops!
[ ...thumbsDown ].length;       // 2 -- oops!
```

Như bạn có thể thấy, đây là hai điểm mã riêng biệt (không phải là một cặp thay thế) mà, nhờ thứ tự và sự liền kề của chúng, khiến kết xuất Unicode của máy tính vẽ biểu tượng ngón tay cái hướng xuống nhưng với tông màu da tối hơn mặc định của nó. Độ dài chuỗi được tính toán do đó là `2`.

Sẽ cần sao chép hầu hết logic kết xuất Unicode phức tạp của một nền tảng để có thể nhận dạng các cụm điểm mã như vậy là một "ký tự" duy nhất vì mục đích đếm độ dài. Có các thư viện có mục đích làm như vậy, nhưng chúng không nhất thiết phải hoàn hảo, và chúng đi kèm với một chi phí lớn về mã bổ sung.

| LƯU Ý: |
| :--- |
| Là một người dùng Twitter, bạn có thể mong đợi có thể đặt 280 biểu tượng cảm xúc ngón tay cái hướng xuống vào một tweet, vì nó trông giống như một ký tự duy nhất. Twitter đếm `"👎"` (ngón tay cái hướng xuống mặc định), `"👎🏾"` (ngón tay cái hướng xuống màu da ngăm đen), và thậm chí là `"👩‍👩‍👦‍👦"` (cụm grapheme emoji gia đình) tất cả đều là 2 ký tự mỗi cái, mặc dù độ dài chuỗi tương ứng của chúng (từ quan điểm của JS) là `2`, `4`, và `7`; do đó, bạn chỉ có thể vừa một nửa số lượng emoji (140 thay vì 280) trong một tweet. Thực tế, Twitter đã thực hiện thay đổi này vào năm 2018 để cân bằng cụ thể việc đếm tất cả các ký tự Unicode, ở mức 2 ký tự mỗi biểu tượng. [^TwitterUnicode] Đó là một thay đổi được hoan nghênh đối với người dùng Twitter, đặc biệt là những người muốn sử dụng các ký tự emoji đại diện nhất cho giới tính, tông màu da dự định, v.v. Tuy nhiên, cũng *thật* tò mò rằng Twitter đã chọn đếm tất cả các biểu tượng Unicode/emoji là 2 ký tự mỗi cái, thay vì 1 ký tự (grapheme) trực quan hơn mỗi cái. |

Việc đếm *độ dài* của một chuỗi để phù hợp với trực giác con người của chúng ta là một nhiệm vụ khó khăn đáng kể, có lẽ là một nghệ thuật hơn là một khoa học. Chúng ta có thể nhận được các xấp xỉ chấp nhận được trong nhiều trường hợp, nhưng có rất nhiều trường hợp khác có thể làm rối tung các chương trình của chúng ta.

### Quốc Tế Hóa (i18n) và Bản Địa Hóa (l10n)

Để phục vụ nhu cầu ngày càng tăng đối với các chương trình JS hoạt động như mong đợi trong bất kỳ bối cảnh ngôn ngữ/văn hóa quốc tế nào, ủy ban ECMAScript cũng xuất bản API Quốc Tế Hóa ECMAScript. [^INTLAPI]

Một chương trình JS mặc định theo một ngôn ngữ/khu vực (locale) theo môi trường chạy chương trình (trang trình duyệt web, thể hiện Node, v.v.). Locale có hiệu lực ảnh hưởng đến việc sắp xếp (và so sánh giá trị), định dạng, và một số hành vi được giả định khác. Các hành vi bị thay đổi như vậy có lẽ rõ ràng hơn một chút với chuỗi, nhưng chúng cũng có thể được nhìn thấy với số (và ngày tháng!).

Nhưng các ký tự chuỗi cũng có thể có thông tin ngôn ngữ/khu vực được nhúng trong chúng, điều này được ưu tiên hơn mặc định môi trường. Nếu ký tự chuỗi mơ hồ/được chia sẻ về mặt ngôn ngữ/khu vực của nó (chẳng hạn như `"a"`), cài đặt môi trường mặc định được sử dụng.

Tùy thuộc vào nội dung của chuỗi, nó có thể được diễn giải là được sắp xếp từ trái sang phải (LTR) hoặc phải sang trái (RTL). Như vậy, nhiều phương thức chuỗi mà chúng ta sẽ đề cập sau này sử dụng các mô tả logic trong tên của chúng, như "start" (bắt đầu), "end" (kết thúc), "begin" (bắt đầu), "last" (cuối cùng), thay vì các thuật ngữ định hướng như "left" (trái) và "right" (phải).

Ví dụ, tiếng Do Thái và tiếng Ả Rập đều là các ngôn ngữ RTL phổ biến:

```js
hebrewHello = "\u{5e9}\u{5dc}\u{5d5}\u{5dd}";

console.log(hebrewHello);                       // שלום
```

Lưu ý rằng ký tự được liệt kê đầu tiên trong literal chuỗi (`"\u{5e9}"`) thực sự là ký tự ngoài cùng bên phải khi chuỗi được hiển thị?

Mặc dù tiếng Do Thái là một ngôn ngữ RTL, bạn không thực sự gõ các ký tự trong literal chuỗi theo thứ tự đảo ngược (RTL) theo cách chúng nên được hiển thị. Bạn nhập các ký tự theo thứ tự logic, trong đó vị trí `0` là ký tự đầu tiên, vị trí `1` là ký tự thứ hai, v.v. Lớp hiển thị là nơi các ký tự RTL được đảo ngược để được hiển thị theo đúng thứ tự của chúng.

Điều đó cũng có nghĩa là nếu bạn truy cập `hebrewHello[0]` (hoặc `hebrewHello.charAt(0)`) -- để lấy ký tự tại vị trí `0` -- bạn nhận được `"ש"` vì đó là ký tự đầu tiên về mặt logic của chuỗi, không phải `"ם"` (ký tự cuối cùng về mặt logic của chuỗi). Truy cập vị trí chỉ số tuân theo vị trí logic, không phải vị trí hiển thị.

Đây là cùng một ví dụ trong một ngôn ngữ RTL khác, tiếng Ả Rập:

```js
arabicHello = "\u{631}\u{62d}\u{628}\u{627}";

console.log(arabicHello);                       // رحبا

console.log(arabicHello[0]);                    // ر
```

Các chương trình JS có thể bắt buộc một ngôn ngữ/khu vực cụ thể, sử dụng các API `Intl` khác nhau như `Intl.Collator`: [^INTLCollator]

```js
germanStringSorter = new Intl.Collator("de");

listOfGermanWords = [ /* .. */ ];

germanStringSorter.compare("Hallo","Welt");
// -1 (hoặc số âm)

// ví dụ phỏng theo MDN:
//
germanStringSorter.compare("Z","z");
// 1 (hoặc số dương)

caseFirstSorter = new Intl.Collator("de",{ caseFirst: "upper", });
caseFirstSorter.compare("Z","z");
// -1 (hoặc số âm)
```

Chuỗi nhiều từ có thể được phân đoạn (segmented) bằng cách sử dụng `Intl.Segmenter`: [^INTLSegmenter]

```js
arabicHelloWorld = "\u{645}\u{631}\u{62d}\u{628}\u{627} \
\u{628}\u{627}\u{644}\u{639}\u{627}\u{644}\u{645}";

console.log(arabicHelloWorld);      // مرحبا بالعالم

arabicSegmenter = new Intl.Segmenter("ar",{ granularity: "word" });

for (
    let { segment: word, isWordLike } of
    arabicSegmenter.segment(arabicHelloWorld)
) {
    if (isWordLike) {
        console.log(word);
    }
}
// مرحبا
//لعالم
```

| LƯU Ý: |
| :--- |
| Phương thức `segment(..)` (từ các thể hiện của `Intl.Segmenter`) trả về một iterator JS tiêu chuẩn, mà vòng lặp `for..of` ở đây tiêu thụ. Thêm về các giao thức lặp trong tiêu đề "Đồng Bộ & Bất Đồng Bộ" của bộ sách này. |

### So Sánh Chuỗi (String Comparison)

Các giá trị chuỗi có thể được so sánh (cho cả sự bằng nhau và thứ tự quan hệ) với các giá trị chuỗi khác, sử dụng các toán tử tích hợp khác nhau. Điều quan trọng cần ghi nhớ là các so sánh như vậy nhạy cảm với nội dung chuỗi thực tế, bao gồm đặc biệt là các điểm mã bên dưới từ các ký tự Unicode không phải BPM.

Cả so sánh bằng và so sánh quan hệ đều phân biệt chữ hoa chữ thường, đối với bất kỳ ký tự nào mà chữ hoa và chữ thường được định nghĩa rõ ràng. Để thực hiện các so sánh không phân biệt chữ hoa chữ thường, hãy chuẩn hóa chữ hoa/thường của cả hai giá trị trước (với `toUpperCase()` hoặc `toLowerCase()`).

#### Sự Bằng Nhau Của Chuỗi (String Equality)

Các toán tử `===` và `==` (cùng với các đối tác phủ định của chúng tương ứng là `!==` và `!=`) là cách phổ biến nhất để thực hiện các so sánh bằng cho các giá trị nguyên thủy, bao gồm các giá trị chuỗi:

```js
"my name" === "my n\x61me";               // true

"my name" !== String.raw`my n\x61me`;     // true
```

Toán tử `===`[^StrictEquality] -- thường được gọi là "sự bằng nhau nghiêm ngặt" (strict equality) -- trước tiên kiểm tra xem các kiểu có khớp không, và nếu không, trả về `false` ngay lập tức. Nếu các kiểu khớp nhau, thì nó kiểm tra xem các giá trị có giống nhau không; đối với chuỗi, đây là một so sánh từng đơn vị mã, từ đầu đến cuối.

Mặc dù có tên là "nghiêm ngặt", có những sắc thái đối với `===` (như xử lý `-0` và `NaN`), nhưng chúng ta sẽ đề cập đến những điều đó sau.

##### Sự Bằng Nhau Ép Buộc (Coercive Equality)

Ngược lại, toán tử `==`[^LooseEquality] -- thường được gọi là "sự bằng nhau lỏng lẻo" (loose equality) -- thực hiện *sự bằng nhau ép buộc*: nếu các kiểu giá trị của hai toán hạng không khớp, `==` trước tiên ép buộc một hoặc cả hai toán hạng cho đến khi các kiểu giá trị *khớp*, và sau đó nó chuyển giao so sánh nội bộ cho `===`.

Ép buộc (Coercion) là một chủ đề cực kỳ quan trọng -- nó là một phần vốn có của hệ thống kiểu JS, một trong 3 trụ cột của ngôn ngữ -- nhưng chúng ta sẽ chỉ giới thiệu ngắn gọn ở đây trong chương này, và xem xét lại chi tiết sau.

| LƯU Ý: |
| :--- |
| Bạn có thể đã nghe giải thích thường được trích dẫn, nhưng tuy nhiên không chính xác, rằng sự khác biệt giữa `==` và `===` là `==` so sánh các giá trị trong khi `===` so sánh cả giá trị và các kiểu. Không đúng, và bạn có thể tự đọc thông số kỹ thuật để xác minh -- cả hai thuật toán đặc tả `isStrictlyEqual(..)` và `isLooselyEqual(..)` đều được liên kết dưới dạng chú thích trong các đoạn trước. Tuy nhiên, để tóm tắt: cả `==` và `===` đều nhận thức và nhạy cảm với các kiểu của các toán hạng. Nếu các kiểu toán hạng giống nhau, cả hai toán tử thực hiện chính xác cùng một điều theo nghĩa đen; nếu các kiểu khác nhau, `==` buộc ép buộc cho đến khi các kiểu khớp nhau, trong khi `===` trả về `false` ngay lập tức. |

Rất phổ biến khi các nhà phát triển khẳng định rằng toán tử `==` gây nhầm lẫn và quá khó sử dụng mà không có bất ngờ (do đó sự ưu tiên gần như phổ quát cho `===`). Tôi nghĩ điều đó hoàn toàn sai lầm, và thực tế, các nhà phát triển JS nên mặc định sử dụng `==` (và tránh `===` nếu có thể). Nhưng chúng ta cần thảo luận nhiều hơn để hỗ trợ một tuyên bố gây tranh cãi như vậy; hãy giữ lại những phản đối của bạn cho đến khi chúng ta xem xét lại nó sau.

Bây giờ, để có được một số trực giác về bản chất ép buộc của `==`, quan sát soi sáng nhất là nếu các kiểu không khớp, `==` *thích* so sánh số hơn. Điều đó có nghĩa là nó sẽ cố gắng chuyển đổi cả hai toán hạng thành số, và sau đó thực hiện kiểm tra bằng nhau (giống như `===`).

Vì vậy, vì nó liên quan đến cuộc thảo luận hiện tại của chúng ta, sự bằng nhau chuỗi thực tế *chỉ có thể được* kiểm tra nếu cả hai toán hạng đã là chuỗi:

```js
// kiểm tra bằng nhau chuỗi thực tế (thông qua === nội bộ):
"42" == "42";           // true
```

`==` không thực sự thực hiện kiểm tra bằng nhau chuỗi. Nếu các kiểu giá trị toán hạng đều là chuỗi, `==` chỉ chuyển giao so sánh cho `===`. Nếu chúng không phải là cả hai chuỗi, các bước ép buộc trong `==` sẽ giảm so sánh khớp thành số thay vì chuỗi:

```js
// kiểm tra bằng nhau số (không phải chuỗi!):
42 == "42";             // true
```

Chúng ta sẽ đề cập đến sự bằng nhau số sau trong chương này.

##### *Thực Sự* Nghiêm Ngặt Bằng Nhau (*Really* Strict Equality)

Ngoài `==` và `===`, JS cung cấp tiện ích `Object.is(..)`, trả về `true` nếu cả hai đối số *hoàn toàn giống hệt nhau*, và `false` nếu không (không có ngoại lệ hoặc sắc thái):

```js
Object.is("42",42);             // false

Object.is("42","\x34\x32");     // true
```

Vì `===` thêm một dấu `=` vào cuối `==` để làm cho nó nghiêm ngặt hơn về hành vi, tôi nói đùa nửa vời rằng tiện ích `Object.is(..)` giống như một toán tử `====` (thêm dấu `=` thứ tư), cho loại kiểm tra bằng nhau thực-sự-nghiêm-ngặt-không-ngoại-lệ!

Điều đó nói rằng, `===` (và `==` nhờ vào sự ủy quyền nội bộ của nó cho `===`) là *cực kỳ dễ đoán*, không có ngoại lệ kỳ lạ, khi nói đến việc so sánh hai giá trị thực-sự-đã-là-chuỗi. Tôi thực sự khuyên bạn nên sử dụng `==` cho các kiểm tra như vậy (hoặc `===`), và dành `Object.is(..)` cho các trường hợp góc (là số).

#### So Sánh Quan Hệ Chuỗi (String Relational Comparisons)

Ngoài các kiểm tra bằng nhau giữa các chuỗi, JS hỗ trợ các so sánh quan hệ giữa các giá trị nguyên thủy, như chuỗi: `<`, `<=`, `>`, và `>=`.

Các toán tử `<` (nhỏ hơn) và `>` (lớn hơn) so sánh hai giá trị chuỗi theo thứ tự từ điển (lexicographically) -- giống như bạn sắp xếp các từ trong từ điển -- và do đó, khá tự giải thích:

```js
"hello" < "world";          // true
```

| LƯU Ý: |
| :--- |
| Như đã đề cập trước đó, chương trình JS đang chạy có một ngôn ngữ mặc định, và các toán tử này so sánh theo ngôn ngữ đó. |

Giống như `==`, các toán tử `<` và `>` là ép buộc về mặt số học. Bất kỳ giá trị nào không phải là số đều bị ép buộc thành số. Vì vậy, cách duy nhất để thực hiện so sánh quan hệ với chuỗi là đảm bảo cả hai toán hạng đã là giá trị chuỗi.

Có lẽ hơi ngạc nhiên, `<` và `>` không có tương đương so sánh nghiêm ngặt, giống như cách `===` tránh sự ép buộc của `==`. Các toán tử này luôn bị ép buộc (khi các kiểu không khớp), và không có cách nào trong JS để tránh điều đó.

Vì vậy, điều gì xảy ra khi cả hai giá trị là chuỗi *trông giống số*?

```js
"100" < "11";               // true
```

Về mặt số học, tất nhiên, `100` *không nên* nhỏ hơn `11`.

Nhưng các so sánh quan hệ giữa hai chuỗi sử dụng thứ tự từ điển. Vì vậy, ký tự `"0"` thứ hai (trong `"100"`) nhỏ hơn ký tự `"1"` thứ hai (trong `"11"`), và do đó `"100"` sẽ được sắp xếp trong *từ điển* trước `"11"`. Các toán tử quan hệ chỉ ép buộc thành số nếu các kiểu toán hạng chưa phải là chuỗi.

Các toán tử `<=` (nhỏ hơn hoặc bằng) và `>=` (lớn hơn hoặc bằng) thực sự là một cách viết tắt cho một kiểm tra hỗn hợp.

```js
"hello" <= "hello";                             // true
("hello" < "hello") || ("hello" == "hello");    // true

"hello" >= "hello";                             // true
("hello" > "hello") || ("hello" == "hello");    // true
```

| LƯU Ý: |
| :--- |
| Đây là một chút sắc thái thông số kỹ thuật thú vị: JS không thực sự định nghĩa các hoạt động lớn hơn (cho `>`) hoặc lớn hơn hoặc bằng (cho `>=`) cơ bản. Thay vào đó, nó định nghĩa chúng bằng cách đảo ngược các đối số cho các đối tác bổ sung *nhỏ hơn* của chúng. Vì vậy, `x > y` được JS xử lý về cơ bản là `y <= x`, và `x >= y` được JS xử lý về cơ bản là `y < x`. Vì vậy, JS chỉ cần chỉ định cách `<` và `==` hoạt động, và do đó nhận được `>` và `>=` miễn phí! |

##### So Sánh Quan Hệ Nhận Biết Ngôn Ngữ (Locale-Aware Relational Comparisons)

Như tôi đã đề cập một chút trước đây, các toán tử quan hệ giả định và sử dụng ngôn ngữ có hiệu lực hiện tại. Tuy nhiên, đôi khi có thể hữu ích để buộc một ngôn ngữ cụ thể để so sánh (chẳng hạn như khi sắp xếp một danh sách các chuỗi).

JS cung cấp phương thức `localeCompare(..)` trên các chuỗi JS cho mục đích này:

```js
"hello".localeCompare("world");
// -1 (hoặc số âm)

"world".localeCompare("hello","en");
// 1 (hoặc số dương)

"hello".localeCompare("hello","en",{ ignorePunctuation: true });
// 0

// ví dụ từ MDN:
//
// trong tiếng Đức, ä được sắp xếp trước z
"ä".localeCompare("z","de");
// -1 (hoặc số âm)

// trong tiếng Thụy Điển, ä được sắp xếp sau z
"ä".localeCompare("z","sv");
// 1 (hoặc số dương)
```

Các đối số thứ hai và thứ ba tùy chọn cho `localeCompare(..)` kiểm soát ngôn ngữ nào sẽ được sử dụng, thông qua API `Intl.Collator`[^INTLCollatorApi], như đã trình bày trước đó.

Bạn có thể sử dụng `localeCompare(..)` khi sắp xếp một mảng các chuỗi:

```js
studentNames = [
    "Lisa",
    "Kyle",
    "Jason"
];

// Array::sort() sửa đổi mảng tại chỗ
studentNames.sort(function alphabetizeNames(name1,name2){
    return name1.localeCompare(name2);
});

studentNames;
// [ "Jason", "Kyle", "Lisa" ]
```

Nhưng như đã thảo luận trước đó, một cách đơn giản hơn (và hiệu quả hơn một chút khi sắp xếp nhiều chuỗi) là sử dụng trực tiếp `Intl.Collator`:

```js
studentNames = [
    "Lisa",
    "Kyle",
    "Jason"
];

nameSorter = new Intl.Collator("en");

// Array::sort() sửa đổi mảng tại chỗ
studentNames.sort(nameSorter.compare);

studentNames;
// [ "Jason", "Kyle", "Lisa" ]
```

### Nối Chuỗi (String Concatenation)

Hai hoặc nhiều giá trị chuỗi có thể được nối (kết hợp) thành một giá trị chuỗi mới, sử dụng toán tử `+`:

```js
greeting = "Hello, " + "Kyle!";

greeting;               // Hello, Kyle!
```

Toán tử `+` sẽ hoạt động như một phép nối chuỗi nếu một trong hai toán hạng (giá trị ở bên trái hoặc bên phải của toán tử) đã là một chuỗi (ngay cả một chuỗi rỗng `""`).

Nếu một toán hạng là chuỗi và toán hạng kia thì không, toán hạng không phải là chuỗi sẽ được ép buộc thành biểu diễn chuỗi của nó cho mục đích nối:

```js
userCount = 7;

status = "There are " + userCount + " users online";

status;         // There are 7 users online
```

Việc nối chuỗi kiểu này về cơ bản là nội suy dữ liệu vào chuỗi, đây là mục đích chính của template literals (xem Chương 1). Vì vậy, đoạn mã sau sẽ có kết quả tương tự nhưng thường được coi là phương pháp ưu tiên hơn:

```js
userCount = 7;

status = `There are ${userCount} users online`;

status;         // There are 7 users online
```

Các tùy chọn khác để nối chuỗi bao gồm `"one".concat("two","three")` và `[ "one", "two", "three" ].join("")`, nhưng các loại phương pháp này chỉ thích hợp khi số lượng chuỗi cần nối phụ thuộc vào điều kiện thời gian chạy/tính toán. Nếu chuỗi có một tập hợp nội dung cố định/đã biết, như trên, template literals là lựa chọn tốt hơn.

### Các Phương Thức Giá Trị Chuỗi

Các giá trị chuỗi cung cấp một loạt các phương thức cụ thể cho chuỗi bổ sung (dưới dạng thuộc tính):

* `charAt(..)`: tạo ra một giá trị chuỗi mới tại chỉ mục số, tương tự như `[ .. ]`; không giống như `[ .. ]`, kết quả luôn là một chuỗi, hoặc là ký tự tại vị trí `0` (nếu một số hợp lệ nằm ngoài phạm vi chỉ mục), hoặc chuỗi rỗng `""` (nếu thiếu/chỉ mục không hợp lệ)

* `at(..)` tương tự như `charAt(..)`, nhưng các chỉ mục âm đếm ngược từ cuối chuỗi

* `charCodeAt(..)`: trả về đơn vị mã số (xem "Mã Hóa Ký Tự JS" trong Chương 1) tại chỉ mục đã chỉ định

* `codePointAt(..)`: trả về toàn bộ điểm mã bắt đầu tại chỉ mục đã chỉ định; nếu một cặp thay thế được tìm thấy ở đó, toàn bộ ký tự (điểm mã) được trả về

* `substr(..)` / `substring(..)` / `slice(..)`: tạo ra một giá trị chuỗi mới đại diện cho một phạm vi các ký tự từ chuỗi ban đầu; chúng khác nhau ở cách chỉ mục bắt đầu/kết thúc của phạm vi được chỉ định hoặc xác định

* `toUpperCase()`: tạo ra một giá trị chuỗi mới là tất cả các ký tự chữ hoa

* `toLowerCase()`: tạo ra một giá trị chuỗi mới là tất cả các ký tự chữ thường

* `toLocaleUpperCase()` / `toLocaleLowerCase()`: sử dụng ánh xạ ngôn ngữ cho các hoạt động chữ hoa hoặc chữ thường

* `concat(..)`: tạo ra một giá trị chuỗi mới là sự nối của chuỗi ban đầu và tất cả các đối số giá trị chuỗi được truyền vào

* `indexOf(..)`: tìm kiếm một đối số giá trị chuỗi trong chuỗi ban đầu, tùy chọn bắt đầu từ vị trí được chỉ định trong đối số thứ hai; trả về vị trí chỉ mục dựa trên `0` nếu tìm thấy, hoặc `-1` nếu không tìm thấy

* `lastIndexOf(..)`: giống như `indexOf(..)` nhưng, từ cuối chuỗi (phải trong ngôn ngữ LTR, trái trong ngôn ngữ RTL)

* `includes(..)`: tương tự như `indexOf(..)` nhưng trả về kết quả boolean

* `search(..)`: tương tự như `indexOf(..)` nhưng với một khớp biểu thức chính quy (regular-expression) như được chỉ định

* `trimStart()` / `trimEnd()` / `trim()`: tạo ra một giá trị chuỗi mới với khoảng trắng được cắt từ đầu chuỗi (trái trong ngôn ngữ LTR, phải trong ngôn ngữ RTL), hoặc cuối chuỗi (phải trong ngôn ngữ LTR, trái trong ngôn ngữ RTL), hoặc cả hai

* `repeat(..)`: tạo ra một chuỗi mới với giá trị chuỗi ban đầu được lặp lại số lần đã chỉ định

* `split(..)`: tạo ra một mảng các giá trị chuỗi được phân tách tại chuỗi đã chỉ định hoặc ranh giới biểu thức chính quy

* `padStart(..)` / `padEnd(..)`: tạo ra một giá trị chuỗi mới với phần đệm (mặc định là khoảng trắng " ", nhưng có thể được ghi đè) áp dụng cho đầu (trái trong ngôn ngữ LTR, phải trong ngôn ngữ RTL) hoặc cuối (phải trong ngôn ngữ LTR, trái trong ngôn ngữ RTL), sao cho kết quả chuỗi cuối cùng có ít nhất một độ dài đã chỉ định

* `startsWith(..)` / `endsWith(..)`: kiểm tra đầu (trái trong ngôn ngữ LTR, phải trong ngôn ngữ RTL) hoặc cuối (phải trong ngôn ngữ LTR) của chuỗi ban đầu cho đối số giá trị chuỗi; trả về kết quả boolean

* `match(..)` / `matchAll(..)`: trả về kết quả khớp biểu thức chính quy giống mảng so với chuỗi ban đầu

* `replace(..)`: trả về một chuỗi mới với sự thay thế từ chuỗi ban đầu, của một hoặc nhiều lần xuất hiện khớp của khớp biểu thức chính quy đã chỉ định

* `normalize(..)`: tạo ra một chuỗi mới với chuẩn hóa Unicode (xem "Chuẩn Hóa Unicode" trong Chương 1) đã được thực hiện trên nội dung

* `localCompare(..)`: hàm so sánh hai chuỗi theo ngôn ngữ hiện tại (hữu ích cho việc sắp xếp); trả về một số âm (thường là `-1` nhưng không được đảm bảo) nếu giá trị chuỗi ban đầu đứng trước giá trị chuỗi đối số theo từ điển, một số dương (thường là `1` nhưng không được đảm bảo) nếu giá trị chuỗi ban đầu đứng sau giá trị chuỗi đối số theo từ điển, và `0` nếu hai chuỗi giống hệt nhau

* `anchor()`, `big()`, `blink()`, `bold()`, `fixed()`, `fontcolor()`, `fontsize()`, `italics()`, `link()`, `small()`, `strike()`, `sub()`, và `sup()`: về mặt lịch sử, những thứ này hữu ích trong việc tạo các đoạn chuỗi HTML; chúng hiện đã bị phản đối và nên tránh

| CẢNH BÁO: |
| :--- |
| Nhiều phương thức được mô tả ở trên dựa vào chỉ mục vị trí. Như đã đề cập trước đó trong phần "Tính Toán Độ Dài", các vị trí này phụ thuộc vào nội dung bên trong của giá trị chuỗi, điều này có nghĩa là nếu một ký tự Unicode mở rộng hiện diện và chiếm hai khe đơn vị mã, điều đó sẽ được tính là hai vị trí chỉ mục thay vì một. Việc không tính đến các đơn vị mã *decomposed*, các cặp thay thế, và các cụm grapheme là một nguồn lỗi phổ biến trong xử lý chuỗi JS. |

Các phương thức chuỗi này đều có thể được gọi trực tiếp trên một giá trị literal, hoặc trên một biến/thuộc tính đang giữ một giá trị chuỗi. Khi áp dụng, chúng tạo ra một giá trị chuỗi mới thay vì sửa đổi giá trị chuỗi hiện có (vì chuỗi là bất biến):

```js
"all these letters".toUpperCase();      // ALL THESE LETTERS

greeting = "Hello!";
greeting.repeat(2);                     // Hello!Hello!
greeting;                               // Hello!
```

### Tiện Ích `String` Tĩnh (Static `String` Helpers)

Các hàm tiện ích chuỗi sau đây được cung cấp trực tiếp trên đối tượng `String`, thay vì là các phương thức trên các giá trị chuỗi riêng lẻ:

* `String.fromCharCode(..)` / `String.fromCodePoint(..)`: tạo ra một chuỗi từ một hoặc nhiều đối số đại diện cho các đơn vị mã (`fromCharCode(..)`) hoặc toàn bộ điểm mã (`fromCodePoint(..)`)

* `String.raw(..)`: một hàm thẻ template mặc định cho phép nội suy trên một template literal nhưng ngăn chặn các chuỗi thoát ký tự được phân tích cú pháp, vì vậy chúng vẫn ở dạng các ký tự đầu vào riêng lẻ *thô* (raw) từ literal

Hơn nữa, hầu hết các giá trị (đặc biệt là nguyên thủy) có thể được ép buộc rõ ràng thành tương đương chuỗi của chúng bằng cách chuyển chúng cho hàm `String(..)` (không có từ khóa `new`). Ví dụ:

```js
String(true);           // "true"
String(42);             // "42"
String(Infinity);       // "Infinity"
String(undefined);      // "undefined"
```

Chúng ta sẽ đề cập chi tiết hơn nhiều về các loại ép buộc kiểu như vậy trong một chương sau.

## Các Hành Vi Của Số (Number Behaviors)

Số được sử dụng cho nhiều nhiệm vụ khác nhau trong các chương trình của chúng ta, nhưng chủ yếu là cho các tính toán toán học. Hãy chú ý kỹ đến cách các số JS hoạt động, để đảm bảo kết quả như mong đợi.

### Sự Không Chính Xác Của Dấu Phẩy Động (Floating Point Imprecision)

Chúng ta cần xem xét lại cuộc thảo luận của chúng ta về IEEE-754 từ Chương 1.

Một trong những vấn đề cổ điển của bất kỳ hệ thống số IEEE-754 nào trong bất kỳ ngôn ngữ lập trình nào -- KHÔNG CHỈ RIÊNG JS! -- là không phải tất cả các hoạt động và giá trị đều có thể phù hợp gọn gàng vào các biểu diễn IEEE-754.

Minh họa phổ biến nhất là:

```js
point3a = 0.1 + 0.2;
point3b = 0.3;

point3a;                        // 0.30000000000000004
point3b;                        // 0.3

point3a === point3b;            // false <-- oops!
```

Phép toán `0.1 + 0.2` kết thúc bằng việc tạo ra lỗi dấu phẩy động (độ trôi), trong đó giá trị được lưu trữ thực sự là `0.30000000000000004`.

Các biểu diễn bit tương ứng là:

```text
// 0.30000000000000004
00111111110100110011001100110011
00110011001100110011001100110100

// 0.3
00111111110100110011001100110011
00110011001100110011001100110011
```

Nếu bạn nhìn kỹ vào các mẫu bit đó, chỉ có 2 bit cuối cùng khác nhau, từ `00` thành `11`. Nhưng điều đó đủ để hai số đó không bằng nhau!

Một lần nữa, chỉ để củng cố: hành vi này **KHÔNG PHẢI LÀ DUY NHẤT** đối với JS theo bất kỳ cách nào. Đây chính xác là cách bất kỳ ngôn ngữ lập trình tuân thủ IEEE-754 nào sẽ hoạt động trong cùng một kịch bản. Như tôi đã khẳng định ở trên, phần lớn tất cả các ngôn ngữ lập trình đều sử dụng IEEE-754, và do đó tất cả chúng đều sẽ chịu chung số phận này.

Sự cám dỗ để chế giễu JS vì `0.1 + 0.2 !== 0.3` là rất mạnh, tôi biết. Nhưng ở đây nó hoàn toàn sai lầm.

| LƯU Ý: |
| :--- |
| Hầu như tất cả các lập trình viên đều cần biết về IEEE-754 và đảm bảo rằng họ cẩn thận về các loại vấn đề này. Thật đáng kinh ngạc, theo một cách đáng thất vọng, có bao nhiêu ít người trong số họ có bất kỳ ý tưởng nào về cách IEEE-754 hoạt động. Nếu bạn đã dành thời gian đọc và hiểu các khái niệm này cho đến nay, bạn hiện đang ở trong tỷ lệ nhỏ hiếm hoi những người thực sự nỗ lực để hiểu các con số trong chương trình của họ! |

#### Ngưỡng Epsilon

Một lời khuyên phổ biến để giải quyết sự không chính xác của dấu phẩy động như vậy sử dụng giá trị `number` *rất nhỏ* này được định nghĩa bởi JS:

```js
Number.EPSILON;                 // 2.220446049250313e-16
```

*Epsilon* là sự khác biệt nhỏ nhất mà JS có thể biểu diễn giữa `1` và giá trị tiếp theo lớn hơn `1`. Mặc dù giá trị này về mặt kỹ thuật phụ thuộc vào việc triển khai/nền tảng, nhưng nó thường vào khoảng `2.2E-16`, hoặc `2^-52`.

Đối với những người không chú ý đủ kỹ đến các chi tiết ở đây -- bao gồm cả bản thân tôi trong quá khứ! -- thường được giả định rằng bất kỳ độ lệch nào trong độ chính xác dấu phẩy động từ một thao tác đơn lẻ sẽ không bao giờ lớn hơn `Number.EPSILON`. Do đó, về mặt lý thuyết, chúng ta có thể sử dụng `Number.EPSILON` như một giá trị dung sai *rất nhỏ* để đảm bảo so sánh bằng số là *an toàn*:

```js
function safeNumberEquals(a,b) {
    return Math.abs(a - b) < Number.EPSILON;
}

point3a = 0.1 + 0.2;
point3b = 0.3;

// những cái này có "bằng nhau" một cách an toàn không?
safeNumberEquals(point3a,point3b);      // true
```

| CẢNH BÁO: |
| :--- |
| Trong cuốn sách "Các Kiểu & Ngữ Pháp" ấn bản đầu tiên, tôi thực sự đã đề xuất chính xác phương pháp này. Tôi đã sai. Tôi nên nghiên cứu chủ đề kỹ hơn. |

Nhưng, hóa ra, phương pháp này không an toàn chút nào:

```js
point3a = 10.1 + 0.2;
point3b = 10.3;

safeNumberEquals(point3a,point3b);      // false :(
```

Chà... thật đáng tiếc!

Thật không may, `Number.EPSILON` chỉ hoạt động như một ngưỡng lỗi "bằng nhau an toàn" cho một số số/hoạt động nhỏ nhất định, và trong các trường hợp khác, nó quá nhỏ, và mang lại các kết quả âm tính giả.

Bạn có thể chia tỷ lệ `Number.EPSILON` theo một số yếu tố để tạo ra ngưỡng lớn hơn giúp tránh âm tính giả nhưng vẫn lọc ra tất cả độ lệch dấu phẩy động trong chương trình của bạn. Nhưng sử dụng yếu tố nào hoàn toàn là một quyết định thủ công dựa trên độ lớn của các giá trị, và các hoạt động trên chúng, mà chương trình của bạn sẽ đòi hỏi. Không có cách tự động nào để tính toán một ngưỡng đáng tin cậy, phổ quát.

Trừ khi bạn thực sự biết mình đang làm gì, bạn chỉ nên *không* sử dụng phương pháp ngưỡng `Number.EPSILON` này chút nào.

| MẸO: |
| :--- |
| Nếu bạn muốn đọc thêm chi tiết và lời khuyên chắc chắn về chủ đề này, tôi thực sự khuyên bạn nên đọc bài đăng này. [^EpsilonBad] Nhưng nếu chúng ta không thể sử dụng `Number.EPSILON` để tránh những nguy hiểm của độ lệch dấu phẩy động, chúng ta phải làm gì? Nếu bạn có thể tránh dấu phẩy động hoàn toàn bằng cách chia tỷ lệ tất cả các số của mình lên để chúng đều là số nguyên (hoặc bigints) trong khi thực hiện phép toán, hãy làm như vậy. Chỉ xử lý các giá trị thập phân khi bạn phải xuất/biểu diễn một giá trị cuối cùng sau khi tất cả các phép toán đã hoàn tất. Nếu điều đó không khả thi/thực tế, hãy sử dụng thư viện giả lập thập phân độ chính xác tùy ý và tránh hoàn toàn các giá trị `number`. Hoặc thực hiện phép toán của bạn trong một môi trường lập trình bên ngoài khác không dựa trên IEEE-754. |

### So Sánh Số (Numeric Comparison)

Giống như chuỗi, các giá trị số có thể được so sánh (cho cả sự bằng nhau và thứ tự quan hệ) bằng cách sử dụng các toán tử tương tự.

Hãy nhớ rằng bất kể hình thức nào mà giá trị số có khi được chỉ định dưới dạng literal (cơ số 10, bát phân, thập lục phân, số mũ, v.v.), giá trị cơ bản được lưu trữ là những gì sẽ được so sánh. Cũng hãy ghi nhớ các vấn đề không chính xác của dấu phẩy động đã thảo luận trong phần trước, vì các so sánh sẽ nhạy cảm với nội dung nhị phân chính xác.

#### Sự Bằng Nhau Của Số (Numeric Equality)

Giống như chuỗi, các so sánh bằng cho số sử dụng hoặc là các toán tử `==` / `===` hoặc `Object.is(..)`. Cũng hãy nhớ rằng nếu các kiểu của cả hai toán hạng giống nhau, `==` thực hiện giống hệt như `===`.

```js
42 == 42;                   // true
42 === 42;                  // true

42 == 43;                   // false
42 === 43;                  // false

Object.is(42,42);           // true
Object.is(42,43);           // false
```

Đối với sự bằng nhau ép buộc `==` (khi các kiểu toán hạng không khớp), nếu một trong hai toán hạng không phải là giá trị chuỗi, `==` thích kiểm tra bằng nhau số hơn (có nghĩa là cả hai toán hạng đều được ép buộc thành số).

```js
// so sánh số (không phải chuỗi!)
42 == "42";                 // true
```

Trong đoạn mã này, sự bằng nhau ép buộc ép buộc `"42"` thành `42`, không phải ngược lại (`42` thành `"42"`). Khi cả hai kiểu đều là `number`, thì các giá trị của chúng được so sánh cho sự bằng nhau chính xác, giống như `===` sẽ làm.

Hãy nhớ lại rằng JS không phân biệt giữa các giá trị như `42`, `42.0`, và `42.000000`; dưới vỏ bọc, chúng đều giống nhau. Không có gì đáng ngạc nhiên, các kiểm tra bằng nhau `==` và `===` xác minh điều đó:

```js
42 == 42.0;                 // true
42.0 == 42.00000;           // true
42.00 === 42.000;           // true
```

Trực giác bạn có thể có là, nếu hai số theo nghĩa đen giống nhau, chúng bằng nhau. Và đó là cách JS diễn giải nó. Nhưng `0.3` không hoàn toàn giống với kết quả của `0.1 + 0.2`, bởi vì (như chúng ta đã thấy trước đó), cái sau tạo ra một giá trị cơ bản *rất gần* với `0.3`, nhưng không hoàn toàn giống hệt nhau.

Điều thú vị là, hai giá trị *gần nhau đến mức* sự khác biệt của chúng nhỏ hơn ngưỡng `Number.EPSILON`, vì vậy JS không thể thực sự biểu diễn sự khác biệt đó *một cách chính xác*.

Sau đó, bạn có thể nghĩ, ít nhất là một cách không chính thức, rằng các số JS như vậy nên "bằng nhau", vì sự khác biệt giữa chúng quá nhỏ để biểu diễn. Nhưng hãy chú ý: JS *có thể* biểu diễn rằng *có* một sự khác biệt, đó là lý do tại sao bạn thấy số `4` ở cuối cùng của số thập phân khi JS đánh giá `0.1 + 0.2`. Và bạn *có thể* gõ ra literal số `0.00000000000000004` (aka, `4e-17`), là sự khác biệt giữa `0.3` và `0.1 + 0.2`.

Những gì JS không thể làm, với các số dấu phẩy động IEEE-754 của nó, là biểu diễn một số nhỏ như vậy theo một cách *đủ chính xác* để các hoạt động trên nó tạo ra kết quả như mong đợi. Nó quá nhỏ để được biểu diễn đầy đủ và đúng đắn trong kiểu `number` mà JS cung cấp.

Vì vậy `0.1 + 0.2 == 0.3` phân giải thành `false`, bởi vì có một sự khác biệt giữa hai giá trị, mặc dù JS không thể biểu diễn chính xác hoặc làm bất cứ điều gì với một giá trị nhỏ như sự khác biệt đó.

Cũng giống như chúng ta đã thấy với chuỗi, các toán tử `!=` (không bằng ép buộc) và `!==` (không bằng nghiêm ngặt) hoạt động với số. `x != y` về cơ bản là `!(x == y)`, và `x !== y` về cơ bản là `!(x === y)`.

Có hai ngoại lệ khó chịu trong sự bằng nhau số (cho dù bạn sử dụng `==` hay `===`):

```js
NaN === NaN;                // false -- ugh!
-0 === 0;                   // true -- ugh!
```

`NaN` không bao giờ bằng chính nó (ngay cả với `===`), và `-0` luôn bằng `0` (ngay cả với `===`). Đôi khi mọi người ngạc nhiên rằng ngay cả `===` cũng có hai ngoại lệ này trong đó.

Tuy nhiên, kiểm tra bằng nhau `Object.is(..)` không có ngoại lệ nào trong số này, vì vậy đối với các so sánh bằng với `NaN` và `-0`, hãy tránh các toán tử `==` / `===` và sử dụng `Object.is(..)` -- hoặc đối với `NaN` cụ thể, `Number.isNaN(..)`.

#### So Sánh Quan Hệ Số

Cũng giống như với các giá trị chuỗi, các toán tử quan hệ JS (`<`, `<=`, `>`, và `>=`) hoạt động với số. Các phép toán `<` (nhỏ hơn) và `>` (lớn hơn) nên khá tự giải thích:

```js
41 < 42;                    // true

0.1 + 0.2 > 0.3;            // true (ugh, IEEE-754)
```

Hãy nhớ: giống như `==`, các toán tử `<` và `>` cũng là ép buộc, có nghĩa là bất kỳ giá trị nào không phải là số đều được ép buộc thành số -- trừ khi cả hai toán hạng đã là chuỗi, như chúng ta đã thấy trước đó. Không có toán tử so sánh quan hệ nghiêm ngặt nào.

Nếu bạn đang thực hiện so sánh quan hệ giữa các số, cách duy nhất để tránh ép buộc là đảm bảo rằng các so sánh luôn có hai số. Nếu không, các toán tử này sẽ thực hiện các so sánh *quan hệ ép buộc* tương tự như cách `==` thực hiện các so sánh *bằng nhau ép buộc*.

### Các Toán Tử Toán Học (Mathematical Operators)

Như tôi đã khẳng định trước đó, lý do chính để có các số trong một ngôn ngữ lập trình là để thực hiện các phép toán toán học với chúng. Vì vậy, hãy nói về cách chúng ta làm điều đó.

Các toán tử số học cơ bản là `+` (cộng), `-` (trừ), `*` (nhân), và `/` (chia). Cũng có sẵn là các toán tử `**` (lũy thừa) và `%` (modulo, hay còn gọi là *phần dư phép chia*). Cũng có các dạng `+=`, `-=`, `*=`, `/=`, `**=`, và `%=` của các toán tử, các dạng này gán thêm kết quả trở lại toán hạng bên trái -- phải là một mục tiêu gán hợp lệ như một biến hoặc thuộc tính.

| LƯU Ý: |
| :--- |
| Như chúng ta đã thấy, toán tử `+` được nạp chồng (overloaded) để hoạt động với cả số và chuỗi. Khi một hoặc cả hai toán hạng là một chuỗi, kết quả là một phép nối chuỗi (bao gồm cả việc ép buộc một trong hai toán hạng thành một chuỗi nếu cần thiết). Nhưng nếu không có toán hạng nào là chuỗi, kết quả là một phép cộng số, như mong đợi. |

Tất cả các toán tử toán học này là *nhị phân* (binary), có nghĩa là chúng mong đợi hai toán hạng giá trị, mỗi cái ở một bên của toán tử; tất cả chúng đều mong đợi các toán hạng là giá trị số. Nếu một trong hai hoặc cả hai toán hạng là không phải số, (các) toán hạng không phải số được ép buộc thành số để thực hiện phép toán. Chúng ta sẽ đề cập chi tiết về sự ép buộc trong một chương sau.

Hãy xem xét:

```js
40 + 2;                 // 42
44 - 2;                 // 42
21 * 2;                 // 42
84 / 2;                 // 42
7 ** 2;                 // 49
49 % 2;                 // 1

40 + "2";               // "402" (nối chuỗi)
44 - "2";               // 42 (vì "2" được ép buộc thành 2)
21 * "2";               // 42 (..ditto..)
84 / "2";               // 42 (..ditto..)
"7" ** "2";             // 49 (cả hai toán hạng được ép buộc thành số)
"49" % "2";             // 1 (..ditto..)
```

Các toán tử `+` và `-` cũng có dạng *đơn nguyên* (unary), có nghĩa là chúng chỉ có một toán hạng; một lần nữa, toán hạng được mong đợi là một số, và được ép buộc thành một số nếu không phải:

```js
+42;                    // 42
-42;                    // -42

+"42";                  // 42
-"42";                  // -42
```

Bạn có thể đã nhận thấy rằng `-42` trông giống như nó chỉ là một literal số "âm bốn mươi hai". Điều đó không hoàn toàn đúng. Một sắc thái của cú pháp JS là nó không nhận ra các literal số âm. Thay vào đó, JS coi đây là một literal số dương `42` được đặt trước, và bị phủ định, bởi toán tử đơn nguyên `-` ở phía trước nó.

Hơi ngạc nhiên, sau đó:

```js
-42;                    // -42
- 42;                   // -42
-
    42;                 // -42
```

Như bạn có thể thấy, khoảng trắng (và thậm chí dòng mới) được phép giữa toán tử đơn nguyên `-` và toán hạng của nó; thực tế, điều này đúng với tất cả các toán tử và toán hạng.

#### Tăng và Giảm (Increment and Decrement)

Có hai toán tử số học đơn nguyên khác: `++` (tăng) và `--` (giảm). Cả hai đều thực hiện phép toán tương ứng của mình và sau đó gán lại kết quả cho toán hạng -- phải là một mục tiêu gán hợp lệ như một biến hoặc thuộc tính.

Bạn có thể nghĩ về `++` tương đương với `+= 1`, và `--` tương đương với `-= 1`:

```js
myAge = 42;

myAge++;
myAge;                  // 43

numberOfHeadHairs--;
```

Tuy nhiên, đây là các toán tử đặc biệt ở chỗ chúng có thể xuất hiện ở vị trí hậu tố (sau toán hạng), như trên, hoặc ở vị trí tiền tố (trước toán hạng):

```js
myAge = 42;

++myAge;
myAge;                  // 43

--numberofHeadHairs;
```

Có vẻ kỳ lạ là các vị trí tiền tố và hậu tố dường như cho cùng một kết quả (tăng hoặc giảm) trong các ví dụ như vậy. Sự khác biệt là tinh tế, và không liên quan đến kết quả được gán lại cuối cùng. Chúng ta sẽ xem xét lại các toán tử cụ thể này trong một chương sau để tìm hiểu sâu về sự khác biệt vị trí.

### Các Toán Tử Bitwise (Bitwise Operators)

JS cung cấp một số toán tử bitwise để thực hiện các thao tác cấp bit trên các giá trị số.

Tuy nhiên, các thao tác bit này không được thực hiện đối với mẫu bit đã đóng gói của các số IEEE-754 (xem Chương 1). Thay vào đó, số toán hạng trước tiên được chuyển đổi thành *số nguyên* có dấu 32-bit, thao tác bit được thực hiện, và sau đó kết quả được chuyển đổi trở lại thành số IEEE-754.

Hãy nhớ rằng, giống như bất kỳ toán tử nguyên thủy nào khác, chúng chỉ tính toán các giá trị mới, không thực sự sửa đổi một giá trị tại chỗ.

* `&` (bitwise AND): Thực hiện một phép toán AND với mỗi bit tương ứng từ hai toán hạng; `42 & 36 === 32` (tức là, `0b00...101010 & 0b00...100100 === 0b00..100000`)

* `|` (bitwise OR): Thực hiện một phép toán OR với mỗi bit tương ứng từ hai toán hạng; `42 | 36 === 46` (tức là, `0b00...101010 | 0b00...100100 === 0b00...101110`)

* `^` (bitwise XOR): Thực hiện một phép toán XOR (eXclusive-OR) với mỗi bit tương ứng từ hai toán hạng; `42 ^ 36 === 14` (tức là, `0b00...101010 ^ 0b00...100100 === 0b00...001110`)

* `~` (bitwise NOT): Thực hiện một phép toán NOT đối với các bit của một toán hạng duy nhất; `~42 === -43` (tức là, `~0b00...101010 === 0b11...010101`); sử dụng bù 2 (2's complement), số nguyên có dấu có bit đầu tiên được đặt thành `1` có nghĩa là âm, và phần còn lại của các bit (khi lật lại, theo bù 2, là lật bit bù 1 và sau đó cộng thêm `1`) sẽ là `43` (`0b10...101011`); tương đương của `~` trong số học thập phân là `~x === -(x + 1)`, vì vậy `~42 === -43`

* `<<` (dịch trái - left shift): Thực hiện dịch trái các bit của toán hạng trái theo số lượng bit được chỉ định bởi toán hạng phải; `42 << 3 == 336` (tức là, `0b00...101010 << 3 === 0b00...101010000`)

* `>>` (dịch phải - right shift): Thực hiện dịch phải lan truyền dấu (sign-propagating) các bit của toán hạng trái theo số lượng bit được chỉ định bởi toán hạng phải, loại bỏ các bit rơi ra khỏi phía bên phải; bất kể bit ngoài cùng bên trái là gì (`0`, hoặc `1` là âm) được sao chép vào dưới dạng các bit ở bên trái (do đó bảo toàn dấu của giá trị ban đầu trong kết quả); `42 >> 3 === 5` (tức là, `0b00..101010 >> 3 === 0b00...000101`)

* `>>>` (dịch phải điền không - zero-fill right shift, hay còn gọi là dịch phải không dấu): Thực hiện dịch phải tương tự như `>>`, nhưng điền `0` vào các bit được dịch vào từ phía bên trái thay vì sao chép bit ngoài cùng bên trái (do đó bỏ qua dấu của giá trị ban đầu trong kết quả); `42 >>> 3 === 5` nhưng `-43 >>> 3 === 536870906` (tức là, `0b11...010101 >>> 3 === 0b0001...111010`)

* `&=`, `|=`, `<<=`, `>>=`, và `>>>=` (toán tử bitwise với phép gán): Thực hiện thao tác bitwise tương ứng, nhưng sau đó gán kết quả cho toán hạng bên trái (phải là một mục tiêu gán hợp lệ, như một biến hoặc thuộc tính, không chỉ là một giá trị literal); lưu ý rằng `~=` bị thiếu trong danh sách, vì không có toán tử "phủ định nhị phân với phép gán" như vậy

Thực lòng mà nói, các thao tác bitwise không phổ biến lắm trong JS. Nhưng đôi khi bạn có thể thấy một câu lệnh như:

```js
myGPA = 3.54;

myGPA | 0;              // 3
```

Vì các toán tử bitwise chỉ hoạt động trên các số nguyên 32-bit, phép toán `| 0` cắt bớt (tức là, `Math.trunc(..)`) bất kỳ giá trị thập phân nào, chỉ để lại số nguyên.

| CẢNH BÁO: |
| :--- |
| Một quan niệm sai lầm phổ biến là `\| 0` giống như *floor* (làm tròn xuống) (tức là, `Math.floor(..)`). Kết quả của `\| 0` đồng ý với `Math.floor(..)` trên các số dương, nhưng khác nhau trên các số âm, vì theo định nghĩa chuẩn, *floor* là một phép toán làm tròn xuống về phía `-Infinity`. `\| 0` chỉ đơn thuần loại bỏ các bit thập phân, thực tế là cắt bớt (truncation). |

### Các Phương Thức Giá Trị Số

Các giá trị số cung cấp các phương thức sau (như các thuộc tính) cho các hoạt động cụ thể của số:

* `toExponential(..)`: tạo ra một biểu diễn chuỗi của số bằng cách sử dụng ký hiệu khoa học (ví dụ, `"4.2e+1"`)

* `toFixed(..)`: tạo ra một biểu diễn chuỗi không phải ký hiệu khoa học của số với số lượng chữ số thập phân được chỉ định (làm tròn hoặc đệm không nếu cần thiết)

* `toPrecision(..)`: giống như `toFixed(..)`, ngoại trừ nó áp dụng đối số số làm số lượng chữ số có nghĩa (tức là, độ chính xác) bao gồm cả số nguyên và vị trí thập phân nếu có

* `toLocaleString(..)`: tạo ra một biểu diễn chuỗi của số theo ngôn ngữ hiện tại

```js
myAge = 42;

myAge.toExponential(3);         // "4.200e+1"
```

Một sắc thái cụ thể của cú pháp JS là `.` có thể mơ hồ khi xử lý các literal số và truy cập thuộc tính/phương thức.

Nếu một dấu `.` xuất hiện ngay lập tức (không có khoảng trắng) sau một chữ số literal, và chưa có dấu `.` thập phân nào trong giá trị số, dấu `.` được giả định là bắt đầu phần thập phân của số. Nhưng nếu vị trí của dấu `.` rõ ràng *không phải* là một phần của literal số, thì nó luôn được coi là một truy cập thuộc tính.

```js
42 .toExponential(3);           // "4.200e+1"
```

Ở đây, khoảng trắng làm rõ dấu `.`, chỉ định nó là một truy cập thuộc tính/phương thức. Có lẽ phổ biến hơn/được ưu tiên hơn là sử dụng `(..)` thay vì khoảng trắng cho việc làm rõ như vậy:

```js
(42).toExponential(3);          // "4.200e+1"
```

Một hiệu ứng trông khác thường của quy tắc ngữ pháp phân tích cú pháp JS này:

```js
42..toExponential(3);           // "4.200e+1"
```

Được gọi là thành ngữ "dấu chấm đôi", dấu `.` đầu tiên trong biểu thức này là một số thập phân, và do đó dấu `.` thứ hai rõ ràng *không phải* là một số thập phân, mà là một truy cập thuộc tính/phương thức.

Ngoài ra, hãy chú ý không có chữ số nào sau dấu `.` đầu tiên; cú pháp hoàn toàn hợp lệ để để lại một dấu `.` ở cuối trên một literal số:

```js
myAge = 41. + 1.;

myAge;                          // 42
```

Các giá trị của kiểu `bigint` không thể có số thập phân, vì vậy việc phân tích cú pháp là rõ ràng rằng một dấu `.` sau một literal (với hậu tố `n`) luôn là một truy cập thuộc tính:

```js
42n.toString();                 // 42
```

### Các Thuộc Tính `Number` Tĩnh

* `Number.EPSILON`: Giá trị nhỏ nhất có thể giữa `1` và số cao nhất tiếp theo

* `Number.NaN`: Giống như symbol `NaN` toàn cục, số không hợp lệ đặc biệt

* `Number.MIN_SAFE_INTEGER` / `Number.MAX_SAFE_INTEGER`: Các số nguyên dương và âm có giá trị tuyệt đối lớn nhất (xa nhất từ `0`)

* `Number.MIN_VALUE` / `Number.MAX_VALUE`: Tối thiểu (giá trị dương gần nhất với `0`) và tối đa (giá trị dương xa nhất từ `0`) có thể biểu diễn bởi kiểu `number`

* `Number.NEGATIVE_INFINITY` / `Number.POSITIVE_INFINITY`: Giống như toàn cục `-Infinity` và `Infinity`, các giá trị đại diện cho các giá trị lớn nhất (không hữu hạn) xa nhất từ `0`

### Tiện Ích `Number` Tĩnh

* `Number.isFinite(..)`: trả về một boolean cho biết giá trị có phải là hữu hạn không -- một `number` không phải là `NaN`, cũng không phải là một trong hai vô cực

* `Number.isInteger(..)` / `Number.isSafeInteger(..)`: cả hai đều trả về boolean cho biết giá trị có phải là một `number` nguyên không có vị trí thập phân, và liệu nó có nằm trong phạm vi *an toàn* cho các số nguyên hay không (`-2^53 + 1` - `2^53 - 1`)

* `Number.isNaN(..)`: Phiên bản đã sửa lỗi của tiện ích `isNaN(..)` toàn cục, xác định xem đối số được cung cấp có phải là giá trị `NaN` đặc biệt hay không

* `Number.parseFloat(..)` / `Number.parseInt(..)`: các tiện ích để phân tích cú pháp các giá trị chuỗi cho các chữ số, từ trái sang phải, cho đến khi gặp kết thúc của chuỗi hoặc ký tự không phải float (hoặc không phải integer) đầu tiên

### Không Gian Tên `Math` Tĩnh

Vì việc sử dụng chính các giá trị `number` là để thực hiện các phép toán toán học, JS bao gồm nhiều hằng số toán học tiêu chuẩn và các tiện ích hoạt động trên không gian tên `Math`.

Có rất nhiều thứ này, vì vậy tôi sẽ bỏ qua việc liệt kê từng cái một. Nhưng đây là một vài cái cho mục đích minh họa:

```js
Math.PI;                        // 3.141592653589793

// giá trị tuyệt đối
Math.abs(-32.6);                // 32.6

// làm tròn
Math.round(-32.6);              // -33

// lựa chọn min/max
Math.min(100,Math.max(0,42));   // 42
```

Không giống như `Number`, cũng là hàm `Number(..)` (cho việc ép buộc số), `Math` chỉ là một đối tượng giữ các thuộc tính và các tiện ích hàm tĩnh này; nó không thể được gọi như một hàm.

| CẢNH BÁO: |
| :--- |
| Một thành viên kỳ lạ của không gian tên `Math` là `Math.random()`, để tạo ra một giá trị dấu phẩy động ngẫu nhiên giữa `0` và `1.0`. Thật bất thường khi coi việc tạo số ngẫu nhiên -- một nhiệm vụ vốn có trạng thái/tác dụng phụ -- như một phép toán toán học. Nó cũng từ lâu đã là một nguy cơ bảo mật, vì bộ tạo số giả ngẫu nhiên (PRNG) mà JS sử dụng *không* an toàn (có thể dự đoán được) từ góc độ mật mã học. Nền tảng web đã can thiệp vài năm trước với API `crypto.getRandomValues(..)` an toàn hơn (dựa trên PRNG tốt hơn), điền vào một typed-array với các bit ngẫu nhiên có thể được diễn giải là một hoặc nhiều số nguyên (có độ lớn tối đa được chỉ định theo kiểu). Việc sử dụng `Math.random()` hiện bị không khuyến khích rộng rãi. |

### BigInts và Numbers Không Trộn Lẫn

Như chúng ta đã đề cập trong Chương 1, các giá trị của kiểu `number` và kiểu `bigint` không thể trộn lẫn trong cùng một phép toán. Điều đó có thể khiến bạn vấp ngã ngay cả khi bạn đang thực hiện một phép tăng đơn giản của giá trị (như trong một vòng lặp):

```js
myAge = 42n;

myAge + 1;                  // TypeError thrown!
myAge += 1;                 // TypeError thrown!

myAge + 1n;                 // 43n
myAge += 1n;                // 43n

myAge++;
myAge;                      // 44n
```

Như vậy, nếu bạn đang sử dụng cả hai giá trị `number` và `bigint` trong các chương trình của mình, bạn sẽ cần phải ép buộc thủ công một kiểu giá trị sang kiểu kia một cách khá thường xuyên. Hàm `BigInt(..)` (không có từ khóa `new`) có thể ép buộc một giá trị `number` thành `bigint`. Ngược lại, để đi theo hướng khác từ `bigint` sang `number`, hãy sử dụng hàm `Number(..)` (một lần nữa, không có từ khóa `new`):

```js
BigInt(42);                 // 42n

Number(42n);                // 42
```

Tuy nhiên, hãy nhớ rằng: việc ép buộc giữa các kiểu này có một số rủi ro:

```js
BigInt(4.2);                // RangeError thrown!
BigInt(NaN);                // RangeError thrown!
BigInt(Infinity);           // RangeError thrown!

Number(2n ** 1024n);        // Infinity
```

## Các Nguyên Thủy Là Nền Tảng

Trong hai chương vừa qua, chúng ta đã đào sâu vào cách các giá trị nguyên thủy hoạt động trong JS. Tôi cá là không ít độc giả, giống như tôi, đã sẵn sàng bỏ qua những chủ đề này. Nhưng bây giờ, hy vọng, bạn thấy tầm quan trọng của việc hiểu những khái niệm này.

Câu chuyện không kết thúc ở đây, mặc dù vậy. Còn xa mới kết thúc! Trong chương tiếp theo, chúng ta sẽ chuyển sự chú ý sang việc hiểu các kiểu đối tượng của JS (đối tượng, mảng, v.v.).

[^TwitterUnicode]: "Cập nhật mới cho thư viện Twitter-Text: Đếm ký tự Emoji"; Andy Piper; Tháng 10 2018; <https://twittercommunity.com/t/new-update-to-the-twitter-text-library-emoji-character-count/114607> ; Truy cập Tháng 7 2022

[^INTLAPI]: Đặc tả API Quốc tế hóa ECMAScript 2022; <https://402.ecma-international.org/9.0/> ; Truy cập Tháng 8 2022

[^INTLCollator]: "Intl.Collator", MDN; <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Collator> ; Truy cập Tháng 8 2022

[^INTLSegmenter]: "Intl.Segmenter", MDN; <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Segmenter> ; Truy cập Tháng 8 2022

[^StrictEquality]: "7.2.16 IsStrictlyEqual(x,y)", Đặc tả ngôn ngữ ECMAScript 2022; <https://262.ecma-international.org/13.0/#sec-isstrictlyequal> ; Truy cập Tháng 8 2022

[^LooseEquality]: "7.2.15 IsLooselyEqual(x,y)", Đặc tả ngôn ngữ ECMAScript 2022; <https://262.ecma-international.org/13.0/#sec-islooselyequal> ; Truy cập Tháng 8 2022

[^EpsilonBad]: "LÀM ƠN đừng làm theo công thức mã trong câu trả lời được chấp nhận", Stack Overflow; Daniel Scott; Tháng 7 2019; <https://stackoverflow.com/a/56967003/228852> ; Truy cập Tháng 8 2022
