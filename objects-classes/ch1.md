# You Don't Know JS Yet: Đối tượng & Lớp - Ấn bản thứ 2
# Chương 1: Nền tảng Đối tượng

| LƯU Ý: |
| :--- |
| Đang trong quá trình thực hiện |

> Mọi thứ trong JS đều là một đối tượng.

Đây là một trong những "sự thật" phổ biến nhất, nhưng cũng sai lầm nhất, thường xuyên được lan truyền về JS. Hãy bắt đầu phá bỏ huyền thoại này.

JS chắc chắn có các đối tượng, nhưng điều đó không có nghĩa là tất cả các giá trị đều là đối tượng. Tuy nhiên, các đối tượng được cho là loại giá trị quan trọng nhất (và đa dạng nhất!) trong ngôn ngữ, vì vậy việc nắm vững chúng là rất quan trọng đối với hành trình JS của bạn.

Cơ chế đối tượng chắc chắn là loại thùng chứa linh hoạt và mạnh mẽ nhất -- thứ mà bạn đặt các giá trị khác vào; mọi chương trình JS bạn viết sẽ sử dụng chúng theo cách này hay cách khác. Nhưng đó không phải là lý do tại sao các đối tượng xứng đáng được ưu tiên hàng đầu cho cuốn sách này. Các đối tượng là nền tảng cho trụ cột thứ hai trong ba trụ cột của JS: prototype (nguyên mẫu).

Tại sao các prototype (cùng với từ khóa `this`, được đề cập sau trong cuốn sách) lại cốt lõi đối với JS đến mức trở thành một trong ba trụ cột của nó? Trong số những thứ khác, các prototype là cách hệ thống đối tượng của JS có thể thể hiện mẫu thiết kế lớp (class design pattern), một trong những mẫu thiết kế được dựa vào rộng rãi nhất trong tất cả các lập trình.

Vì vậy, hành trình của chúng ta ở đây sẽ bắt đầu với các đối tượng, xây dựng một sự hiểu biết hoàn chỉnh về các prototype, làm sáng tỏ từ khóa `this`, và khám phá hệ thống `class`.

## Về cuốn sách này

Chào mừng bạn đến với cuốn sách thứ 3 trong bộ *You Don't Know JS Yet*! Nếu bạn đã hoàn thành *Bắt đầu* (cuốn sách đầu tiên) và *Phạm vi & Closures* (cuốn sách thứ hai), bạn đang ở đúng nơi! Nếu chưa, trước khi bạn tiếp tục, tôi khuyến khích bạn đọc hai cuốn đó làm nền tảng trước khi đi sâu vào cuốn sách này.

Ấn bản đầu tiên của cuốn sách này có tiêu đề, "this & Object Prototypes". Trong cuốn sách đó, trọng tâm của chúng tôi bắt đầu với từ khóa `this`, vì nó được cho là một trong những chủ đề gây nhầm lẫn nhất trong tất cả JS. Cuốn sách sau đó dành phần lớn thời gian tập trung vào việc giải thích hệ thống prototype và ủng hộ việc nắm lấy mẫu "ủy quyền" (delegation) ít được biết đến hơn thay vì các thiết kế lớp. Vào thời điểm viết cuốn sách đó (2014), ES6 vẫn còn gần 2 năm nữa mới hoàn thành, vì vậy tôi cảm thấy những phác thảo ban đầu của từ khóa `class` chỉ xứng đáng được đề cập ngắn gọn trong phần phụ lục.

Thật là nói giảm nói tránh khi nói rằng rất nhiều thứ đã thay đổi trong bối cảnh JS trong gần 8 năm kể từ cuốn sách đó. ES6 bây giờ là tin cũ; tại thời điểm viết cuốn sách *này*, JS đã thấy 7 bản cập nhật hàng năm **sau ES6** (ES2016 đến ES2022).

Bây giờ, chúng ta vẫn cần nói về cách `this` hoạt động, và cách nó liên quan đến các phương thức được gọi đối với các đối tượng khác nhau. Và `class` thực sự hoạt động (hầu hết!) thông qua chuỗi prototype sâu bên dưới lớp vỏ. Nhưng các nhà phát triển JS vào năm 2022 hầu như không bao giờ viết mã để nối dây thừa kế prototype một cách rõ ràng nữa. Và mặc dù cá nhân tôi mong muốn khác đi, các mẫu thiết kế lớp -- không phải "ủy quyền hành vi" -- là cách phần lớn tổ chức dữ liệu và hành vi (cấu trúc dữ liệu) trong JS được thể hiện.

Cuốn sách này phản ánh thực tế hiện tại của JS: do đó có tiêu đề phụ mới, tổ chức và trọng tâm chủ đề mới, và viết lại hoàn toàn văn bản của ấn bản trước.

## Đối tượng như là Thùng chứa

Một cách phổ biến để thu thập nhiều giá trị trong một thùng chứa duy nhất là với một đối tượng. Các đối tượng là tập hợp các cặp khóa/giá trị. Cũng có các kiểu con của đối tượng trong JS với các hành vi chuyên biệt, chẳng hạn như mảng (được lập chỉ mục bằng số) và thậm chí các hàm (có thể gọi được); thêm về các kiểu con này sau.

| LƯU Ý: |
| :--- |
| Các khóa thường được gọi là "tên thuộc tính", với việc ghép nối tên thuộc tính và giá trị thường được gọi là "thuộc tính". Cuốn sách này sẽ sử dụng các thuật ngữ đó một cách riêng biệt theo cách đó. |

Các đối tượng JS thông thường thường được khai báo bằng cú pháp literal (nguyên văn), như thế này:

```js
myObj = {
    // ..
};
```

**Lưu ý:** Có một cách thay thế để tạo một đối tượng (sử dụng `myObj = new Object()`), nhưng cách này không phổ biến hoặc được ưa thích, và hầu như không bao giờ là cách thích hợp để thực hiện. Hãy gắn bó với cú pháp object literal.

Rất dễ bị nhầm lẫn ý nghĩa của các cặp `{ .. }`, vì JS nạp chồng các dấu ngoặc nhọn để có nghĩa là bất kỳ điều nào sau đây, tùy thuộc vào ngữ cảnh được sử dụng:

* phân định các giá trị, như object literal
* định nghĩa các mẫu destructuring đối tượng (thêm về điều này sau)
* phân định các biểu thức chuỗi nội suy, như `` `some ${ getNumber() } thing` ``
* định nghĩa các khối, như trên các vòng lặp `if` và `for`
* định nghĩa thân hàm

Mặc dù đôi khi có thể khó khăn khi bạn đọc mã, hãy tìm xem liệu một cặp dấu ngoặc nhọn `{ .. }` có được sử dụng trong chương trình nơi một giá trị/biểu thức hợp lệ để xuất hiện hay không; nếu vậy, đó là một object literal, nếu không thì đó là một trong những cách sử dụng nạp chồng khác.

## Định nghĩa Thuộc tính

Bên trong các dấu ngoặc nhọn object literal, bạn định nghĩa các thuộc tính (tên và giá trị) với các cặp `propertyName: propertyValue`, như thế này:

```js
myObj = {
    favoriteNumber: 42,
    isDeveloper: true,
    firstName: "Kyle"
};
```

Các giá trị bạn gán cho các thuộc tính có thể là literal, như được hiển thị, hoặc có thể được tính toán bằng biểu thức:

```js
function twenty() { return 20; }

myObj = {
    favoriteNumber: (twenty() + 1) * 2,
};
```

Biểu thức `(twenty() + 1) * 2` được đánh giá ngay lập tức, với kết quả (`42`) được gán làm giá trị thuộc tính.

Các nhà phát triển đôi khi tự hỏi liệu có cách nào để định nghĩa một biểu thức cho một giá trị thuộc tính trong đó biểu thức là "lười biếng" (lazy), nghĩa là nó không được tính toán tại thời điểm gán, mà được định nghĩa sau đó. JS không có các biểu thức lười biếng, vì vậy cách duy nhất để làm như vậy là biểu thức được bọc trong một hàm:

```js
function twenty() { return 20; }
function myNumber() { return (twenty() + 1) * 2; }

myObj = {
    favoriteNumber: myNumber   // chú ý, KHÔNG phải `myNumber()` như một cuộc gọi hàm
};
```

Trong trường hợp này, `favoriteNumber` không giữ một giá trị số, mà là một tham chiếu hàm. Để tính toán kết quả, tham chiếu hàm đó phải được thực thi một cách rõ ràng.

### Trông giống JSON?

Bạn có thể nhận thấy rằng cú pháp object-literal mà chúng ta đã thấy cho đến nay giống với một cú pháp liên quan, "JSON" (JavaScript Object Notation):

```json
{
    "favoriteNumber": 42,
    "isDeveloper": true,
    "firstName": "Kyle"
}
```

Sự khác biệt lớn nhất giữa các object literal của JS và JSON là, đối với các đối tượng được định nghĩa là JSON:

1. tên thuộc tính phải được trích dẫn bằng các ký tự ngoặc kép `"`

2. giá trị thuộc tính phải là literal (nguyên thủy, đối tượng hoặc mảng), không phải là các biểu thức JS tùy ý

Trong các chương trình JS, một object literal không yêu cầu tên thuộc tính được trích dẫn -- bạn *có thể* trích dẫn chúng (`'` hoặc `"` đều được phép), nhưng thường là tùy chọn. Tuy nhiên, có những ký tự hợp lệ trong tên thuộc tính, nhưng không thể được bao gồm nếu không có dấu ngoặc kép bao quanh; ví dụ: số đứng đầu hoặc khoảng trắng:

```js
myObj = {
    favoriteNumber: 42,
    isDeveloper: true,
    firstName: "Kyle",
    "2 nicknames": [ "getify", "ydkjs" ]
};
```

Một sự khác biệt nhỏ khác là, cú pháp JSON -- nghĩa là, văn bản sẽ được *phân tích cú pháp* dưới dạng JSON, chẳng hạn như từ tệp `.json` -- nghiêm ngặt hơn JS nói chung. Ví dụ: JS cho phép nhận xét (`// ..` và `/* .. */`), và dấu phẩy `,` ở cuối trong các biểu thức đối tượng và mảng; JSON không cho phép bất kỳ điều nào trong số này. Rất may, JSON vẫn cho phép khoảng trắng tùy ý.

### Tên Thuộc tính

Tên thuộc tính trong object literal hầu như luôn được xử lý/ép kiểu thành giá trị chuỗi. Một ngoại lệ cho điều này là đối với các "tên" thuộc tính số nguyên (hoặc "trông giống số nguyên"):

```js
anotherObj = {
    42:       "<-- tên thuộc tính này sẽ được xử lý như một số nguyên",
    "41":     "<-- ...và cái này cũng vậy",

    true:     "<-- tên thuộc tính này sẽ được xử lý như một chuỗi",
    [myObj]:  "<-- ...và cái này cũng vậy"
};
```

Tên thuộc tính `42` sẽ được xử lý như một tên thuộc tính số nguyên (còn gọi là chỉ mục); giá trị chuỗi `"41"` cũng sẽ được xử lý như vậy vì nó *trông giống* một số nguyên. Ngược lại, giá trị `true` sẽ trở thành tên thuộc tính chuỗi `"true"`, và tham chiếu định danh `myObj`, được *tính toán* thông qua `[ .. ]` bao quanh, sẽ ép kiểu giá trị của đối tượng thành một chuỗi (thường là mặc định `"[object Object]"`).

| CẢNH BÁO: |
| :--- |
| Nếu bạn cần thực sự sử dụng một đối tượng làm tên khóa/thuộc tính, đừng bao giờ dựa vào ép kiểu chuỗi được tính toán này; hành vi của nó gây ngạc nhiên và gần như chắc chắn không phải là những gì được mong đợi, vì vậy lỗi chương trình có khả năng xảy ra. Thay vào đó, hãy sử dụng cấu trúc dữ liệu chuyên biệt hơn, được gọi là `Map` (được thêm vào trong ES6), trong đó các đối tượng được sử dụng làm "tên" thuộc tính được giữ nguyên thay vì bị ép kiểu thành giá trị chuỗi. |

Như với `[myObj]` ở trên, bạn có thể *tính toán* bất kỳ **tên thuộc tính** nào (khác biệt với việc tính toán giá trị thuộc tính) tại thời điểm định nghĩa object literal:

```js
anotherObj = {
    ["x" + (21 * 2)]: true
};
```

Biểu thức `"x" + (21 * 2)`, phải xuất hiện bên trong dấu ngoặc `[ .. ]`, được tính toán ngay lập tức, và kết quả (`"x42"`) được sử dụng làm tên thuộc tính.

### Symbol Làm Tên Thuộc tính

ES6 đã thêm một loại giá trị nguyên thủy mới là `Symbol`, thường được sử dụng làm tên thuộc tính đặc biệt để lưu trữ và truy xuất các giá trị thuộc tính. Chúng được tạo thông qua lệnh gọi hàm `Symbol(..)` (**không có** từ khóa `new`), chấp nhận một chuỗi mô tả tùy chọn chỉ được sử dụng cho mục đích gỡ lỗi thân thiện hơn; nếu được chỉ định, mô tả không thể truy cập được đối với chương trình JS và do đó không được sử dụng cho bất kỳ mục đích nào khác ngoài đầu ra gỡ lỗi.

```js
myPropSymbol = Symbol("mô tả tùy chọn, thân thiện với nhà phát triển");
```

| LƯU Ý: |
| :--- |
| Các Symbol giống như số hoặc chuỗi, ngoại trừ việc giá trị của chúng là *mờ đục* đối với, và duy nhất trên toàn cầu trong, chương trình JS. Nói cách khác, bạn có thể tạo và sử dụng các symbol, nhưng JS không cho bạn biết bất cứ điều gì về, hoặc làm bất cứ điều gì với, giá trị cơ bản; điều đó được giữ như một chi tiết triển khai ẩn bởi công cụ JS. |

Tên thuộc tính được tính toán, như đã mô tả trước đây, là cách để định nghĩa tên thuộc tính symbol trên một object literal:

```js
myPropSymbol = Symbol("mô tả tùy chọn, thân thiện với nhà phát triển");

anotherObj = {
    [myPropSymbol]: "Xin chào, symbol!"
};
```

Tên thuộc tính được tính toán được sử dụng để định nghĩa thuộc tính trên `anotherObj` sẽ là giá trị symbol nguyên thủy thực tế (bất kể nó là gì), không phải chuỗi mô tả tùy chọn (`"mô tả tùy chọn, thân thiện với nhà phát triển"`).

Bởi vì các symbol là duy nhất trên toàn cầu trong chương trình của bạn, **không có** cơ hội va chạm ngẫu nhiên khi một phần của chương trình có thể vô tình định nghĩa tên thuộc tính giống như một phần khác của chương trình đã cố gắng định nghĩa/gán.

Các symbol cũng hữu ích để móc nối vào các hành vi mặc định đặc biệt của các đối tượng, và chúng ta sẽ đề cập chi tiết hơn về điều đó trong "Mở rộng MOP" trong chương tiếp theo.

### Thuộc tính Ngắn gọn (Concise Properties)

Khi định nghĩa một object literal, thường sử dụng tên thuộc tính giống với một định danh trong phạm vi hiện có giữ giá trị bạn muốn gán.

```js
coolFact = "người đầu tiên bị kết tội chạy quá tốc độ đã đi với tốc độ 8 dặm/giờ";

anotherObj = {
    coolFact: coolFact
};
```

| LƯU Ý: |
| :--- |
| Điều đó sẽ giống như định nghĩa tên thuộc tính được trích dẫn `"coolFact": coolFact`, nhưng các nhà phát triển JS hiếm khi trích dẫn tên thuộc tính trừ khi thực sự cần thiết. Thật vậy, theo quy ước là tránh các dấu ngoặc kép trừ khi được yêu cầu, vì vậy không nên bao gồm chúng một cách không cần thiết. |

Trong tình huống này, khi tên thuộc tính và định danh biểu thức giá trị giống hệt nhau, bạn có thể bỏ qua phần tên thuộc tính của định nghĩa thuộc tính, như một định nghĩa cái gọi là "thuộc tính ngắn gọn":

```js
coolFact = "người đầu tiên bị kết tội chạy quá tốc độ đã đi với tốc độ 8 dặm/giờ";

anotherObj = {
    coolFact   // <-- viết tắt thuộc tính ngắn gọn
};
```

Tên thuộc tính là `"coolFact"` (chuỗi), và giá trị được gán cho thuộc tính là những gì có trong biến `coolFact` tại thời điểm đó: `"người đầu tiên bị kết tội chạy quá tốc độ đã đi với tốc độ 8 dặm/giờ"`.

Lúc đầu, sự tiện lợi viết tắt này có vẻ khó hiểu. Nhưng khi bạn quen thuộc hơn với việc nhìn thấy tính năng rất phổ biến và được ưa chuộng này được sử dụng, bạn có thể sẽ thích nó vì gõ (và đọc!) ít hơn.

### Phương thức Ngắn gọn (Concise Methods)

Một cách viết tắt tương tự khác là định nghĩa các hàm/phương thức trong một object literal bằng cách sử dụng một dạng ngắn gọn hơn:

```js
anotherObj = {
    // thuộc tính hàm tiêu chuẩn
    greet: function() { console.log("Xin chào!"); },

    // thuộc tính hàm/phương thức ngắn gọn
    greet2() { console.log("Xin chào, bạn!"); }
};
```

Trong khi chúng ta đang nói về chủ đề các thuộc tính phương thức ngắn gọn, chúng ta cũng có thể định nghĩa các hàm generator (một tính năng ES6 khác):

```js
anotherObj = {
    // thay vì:
    //   greet3: function*() { yield "Xin chào, mọi người!"; }

    // phương thức generator ngắn gọn
    *greet3() { yield "Xin chào, mọi người!"; }
};
```

Và mặc dù không đặc biệt phổ biến, các phương thức/generator ngắn gọn thậm chí có thể có tên được trích dẫn hoặc được tính toán:

```js
anotherObj = {
    "greet-4"() { console.log("Xin chào, khán giả!"); },

    // tên được tính toán ngắn gọn
    [ "gr" + "eet 5" ]() { console.log("Xin chào, khán giả!"); },

    // tên generator được tính toán ngắn gọn
    *[ "ok, greet 6".toUpperCase() ]() { yield "Xin chào, khán giả!"; }
};
```

### Object Spread

Một cách khác để định nghĩa các thuộc tính tại thời điểm tạo object literal là với một dạng của cú pháp `...` -- về mặt kỹ thuật nó không phải là một toán tử, nhưng nó chắc chắn có vẻ giống như một toán tử -- thường được gọi là "object spread".

`...` khi được sử dụng bên trong một object literal sẽ "trải" (spread) nội dung (các thuộc tính, hay còn gọi là cặp khóa/giá trị) của một giá trị đối tượng khác vào đối tượng đang được định nghĩa:

```js
anotherObj = {
    favoriteNumber: 12,

    ...myObj,   // object spread, sao chép nông `myObj`

    greeting: "Xin chào!"
}
```

Việc trải các thuộc tính của `myObj` là nông (shallow), ở chỗ nó chỉ sao chép các thuộc tính cấp cao nhất từ `myObj`; bất kỳ giá trị nào mà các thuộc tính đó giữ chỉ đơn giản là được gán qua. Nếu bất kỳ giá trị nào trong số đó là tham chiếu đến các đối tượng khác, bản thân các tham chiếu được gán (bằng cách sao chép), nhưng các giá trị đối tượng cơ bản *không* được sao chép -- vì vậy bạn sẽ có nhiều tham chiếu được chia sẻ đến cùng một đối tượng.

Bạn có thể nghĩ về object spread giống như một vòng lặp `for` chạy qua các thuộc tính từng cái một và thực hiện gán kiểu `=` từ đối tượng nguồn (`myObj`) sang đối tượng đích (`anotherObj`).

Ngoài ra, hãy xem xét các hoạt động định nghĩa thuộc tính này xảy ra "theo thứ tự", từ trên xuống dưới của object literal. Trong đoạn mã trên, vì `myObj` có thuộc tính `favoriteNumber`, object spread sẽ kết thúc bằng việc ghi đè gán thuộc tính `favoriteNumber: 12` từ dòng trước đó. Hơn nữa, nếu `myObj` chứa một thuộc tính `greeting` được sao chép qua, dòng tiếp theo (`greeting: "Xin chào!"`) sẽ ghi đè định nghĩa thuộc tính đó.

| LƯU Ý: |
| :--- |
| Object spread cũng chỉ sao chép các thuộc tính *được sở hữu* (những thuộc tính trực tiếp trên đối tượng) mà *có thể liệt kê* (được phép liệt kê/liệt kê). Nó không sao chép thuộc tính -- như trong, thực sự bắt chước các đặc điểm chính xác của thuộc tính -- mà thay vào đó thực hiện một bản sao kiểu gán đơn giản. Chúng ta sẽ đề cập thêm chi tiết như vậy trong phần "Mô tả Thuộc tính" của chương tiếp theo. |

Một cách phổ biến `...` object spread được sử dụng là để thực hiện sao chép đối tượng *nông*:

```js
myObjShallowCopy = { ...myObj };
```

Hãy nhớ rằng bạn không thể `...` spread vào một giá trị đối tượng hiện có; cú pháp `...` object spread chỉ có thể xuất hiện bên trong object literal `{ .. }`, đang tạo ra một giá trị đối tượng mới. Để thực hiện một bản sao đối tượng nông tương tự nhưng với API thay vì cú pháp, hãy xem phần "Object Entries" sau trong chương này (với phạm vi bao phủ của `Object.entries(..)` và `Object.fromEntries(..)`).

Nhưng nếu thay vào đó bạn muốn sao chép các thuộc tính đối tượng (một cách nông) vào một đối tượng *hiện có*, hãy xem phần "Gán Thuộc tính" sau trong chương này (với phạm vi bao phủ của `Object.assign(..)`).

### Sao chép Đối tượng Sâu (Deep Object Copy)

Ngoài ra, vì `...` không thực hiện sao chép đối tượng đầy đủ, sâu, object spread thường chỉ thích hợp để sao chép các đối tượng giữ các giá trị nguyên thủy, đơn giản, không phải tham chiếu đến các đối tượng khác.

Sao chép đối tượng sâu là một hoạt động vô cùng phức tạp và tinh tế. Sao chép một giá trị như `42` là rõ ràng và đơn giản, nhưng sao chép một hàm (là một loại đối tượng đặc biệt, cũng được giữ bằng tham chiếu), hoặc sao chép một tham chiếu đối tượng bên ngoài (không hoàn toàn trong JS), chẳng hạn như một phần tử DOM có nghĩa là gì? Và điều gì xảy ra nếu một đối tượng có các tham chiếu vòng tròn (như khi một đối tượng con lồng nhau giữ một tham chiếu ngược lên một đối tượng tổ tiên bên ngoài)? Có rất nhiều ý kiến khác nhau trong tự nhiên về cách xử lý tất cả các trường hợp góc này, và do đó không có tiêu chuẩn duy nhất nào tồn tại cho sao chép đối tượng sâu.

Đối với sao chép đối tượng sâu, các phương pháp tiếp cận tiêu chuẩn là:

1. Sử dụng tiện ích thư viện tuyên bố một ý kiến cụ thể về cách xử lý các hành vi/sắc thái sao chép.

2. Sử dụng thủ thuật khứ hồi `JSON.parse(JSON.stringify(..))` -- điều này chỉ "hoạt động" chính xác nếu không có tham chiếu vòng tròn, và nếu không có giá trị nào trong đối tượng không thể được tuần tự hóa đúng cách bằng JSON (chẳng hạn như hàm).

Tuy nhiên, gần đây, một tùy chọn thứ ba đã xuất hiện. Đây không phải là một tính năng JS, mà là một API đồng hành được cung cấp cho JS bởi các môi trường như nền tảng web. Các đối tượng hiện có thể được sao chép sâu bằng cách sử dụng `structuredClone(..)`[^structuredClone].

```js
myObjCopy = structuredClone(myObj);
```

Thuật toán cơ bản đằng sau tiện ích tích hợp này hỗ trợ sao chép các tham chiếu vòng tròn, cũng như **nhiều loại** giá trị hơn so với thủ thuật khứ hồi `JSON`. Tuy nhiên, thuật toán này vẫn có giới hạn của nó, bao gồm không hỗ trợ sao chép hàm hoặc phần tử DOM.

## Truy cập Thuộc tính

Truy cập thuộc tính của một đối tượng hiện có tốt nhất là được thực hiện với toán tử `.`:

```js
myObj.favoriteNumber;    // 42
myObj.isDeveloper;       // true
```

Nếu có thể truy cập một thuộc tính theo cách này, bạn nên làm như vậy.

Nếu tên thuộc tính chứa các ký tự không thể xuất hiện trong định danh, chẳng hạn như số đứng đầu hoặc khoảng trắng, dấu ngoặc `[ .. ]` có thể được sử dụng thay vì `.`:

```js
myObj["2 nicknames"];    // [ "getify", "ydkjs" ]
```

```js
anotherObj[42];          // "<-- tên thuộc tính này sẽ..."
anotherObj["41"];        // "<-- tên thuộc tính này sẽ..."
```

Mặc dù "tên" thuộc tính số vẫn là số, truy cập thuộc tính thông qua dấu ngoặc `[ .. ]` sẽ ép kiểu biểu diễn chuỗi thành số (ví dụ: `"42"` thành tương đương số `42`), và sau đó truy cập thuộc tính số liên quan tương ứng.

Tương tự như object literal, tên thuộc tính để truy cập có thể được tính toán thông qua dấu ngoặc `[ .. ]`. Biểu thức có thể là một định danh đơn giản:

```js
propName = "41";
anotherObj[propName];
```

Thực ra, những gì bạn đặt giữa các dấu ngoặc `[ .. ]` có thể là bất kỳ biểu thức JS tùy ý nào, không chỉ là các định danh hoặc giá trị literal như `42` hoặc `"isDeveloper"`. JS trước tiên sẽ đánh giá biểu thức, và giá trị kết quả sau đó sẽ được sử dụng làm tên thuộc tính để tra cứu trên đối tượng:

```js
function howMany(x) {
    return x + 1;
}

myObj[`${ howMany(1) } nicknames`];   // [ "getify", "ydkjs" ]
```

Trong đoạn mã này, biểu thức là một `` `template string literal` `` được phân định bằng dấu huyền với biểu thức nội suy của lệnh gọi hàm `howMany(1)`. Kết quả tổng thể của biểu thức đó là giá trị chuỗi `"2 nicknames"`, sau đó được sử dụng làm tên thuộc tính để truy cập.

### Object Entries

Bạn có thể nhận được danh sách các thuộc tính trong một đối tượng, dưới dạng một mảng các bộ dữ liệu (mảng con hai phần tử) giữ tên và giá trị thuộc tính:

```js
myObj = {
    favoriteNumber: 42,
    isDeveloper: true,
    firstName: "Kyle"
};

Object.entries(myObj);
// [ ["favoriteNumber",42], ["isDeveloper",true], ["firstName","Kyle"] ]
```

Được thêm vào trong ES6, `Object.entries(..)` truy xuất danh sách các mục này -- chỉ chứa các thuộc tính được sở hữu và có thể liệt kê; xem phần "Mô tả Thuộc tính" trong chương tiếp theo -- từ một đối tượng nguồn.

Một danh sách như vậy có thể được lặp lại, có khả năng gán các thuộc tính cho một đối tượng hiện có khác. Tuy nhiên, cũng có thể tạo một đối tượng mới từ danh sách các mục, sử dụng `Object.fromEntries(..)` (được thêm vào trong ES2019):

```js
myObjShallowCopy = Object.fromEntries( Object.entries(myObj) );

// cách tiếp cận thay thế cho những gì đã thảo luận trước đó:
// myObjShallowCopy = { ...myObj };
```

### Destructuring

Một cách tiếp cận khác để truy cập các thuộc tính là thông qua object destructuring (được thêm vào trong ES6). Hãy nghĩ về destructuring như việc định nghĩa một "mẫu" mô tả giá trị đối tượng được cho là "trông như thế nào" (về mặt cấu trúc), và sau đó yêu cầu JS tuân theo "mẫu" đó để truy cập một cách có hệ thống nội dung của giá trị đối tượng.

Kết quả cuối cùng của object destructuring không phải là một đối tượng khác, mà là một hoặc nhiều phép gán cho các mục tiêu khác (biến, v.v.) của các giá trị từ đối tượng nguồn.

Hãy tưởng tượng loại mã trước ES6 này:

```js
myObj = {
    favoriteNumber: 42,
    isDeveloper: true,
    firstName: "Kyle"
};

const favoriteNumber = (
    myObj.favoriteNumber !== undefined ? myObj.favoriteNumber : 42
);
const isDev = myObj.isDeveloper;
const firstName = myObj.firstName;
const lname = (
    myObj.lastName !== undefined ? myObj.lastName : "--thiếu--"
);
```

Những truy cập giá trị thuộc tính đó, và gán cho các định danh khác, thường được gọi là "destructuring thủ công". Để sử dụng cú pháp object destructuring khai báo, nó có thể trông như thế này:

```js
myObj = {
    favoriteNumber: 42,
    isDeveloper: true,
    firstName: "Kyle"
};

const { favoriteNumber = 12 } = myObj;
const {
    isDeveloper: isDev,
    firstName: firstName,
    lastName: lname = "--thiếu--"
} = myObj;

favoriteNumber;   // 42
isDev;            // true
firstName;        // "Kyle"
lname;            // "--thiếu--"
```

Như đã thấy, object destructuring `{ .. }` giống với định nghĩa giá trị object literal, nhưng nó xuất hiện ở phía bên trái của toán tử `=` thay vì ở phía bên phải nơi biểu thức giá trị đối tượng sẽ xuất hiện. Điều đó làm cho `{ .. }` ở phía bên trái trở thành một mẫu destructuring thay vì một định nghĩa đối tượng khác.

Destructuring `{ favoriteNumber } = myObj` bảo JS tìm một thuộc tính có tên `favoriteNumber` trên đối tượng, và gán giá trị của nó cho một định danh có cùng tên. Trường hợp duy nhất của định danh `favoriteNumber` trong mẫu tương tự như "thuộc tính ngắn gọn" như đã thảo luận trước đó trong chương này: nếu nguồn (tên thuộc tính) và đích (định danh) giống nhau, bạn có thể bỏ qua một trong số chúng và chỉ liệt kê nó một lần.

Phần `= 12` bảo JS cung cấp `12` làm giá trị mặc định cho phép gán cho `favoriteNumber`, nếu đối tượng nguồn không có thuộc tính `favoriteNumber` hoặc nếu thuộc tính giữ giá trị `undefined`.

Trong mẫu destructuring thứ hai, mẫu `isDeveloper: isDev` đang hướng dẫn JS tìm một thuộc tính có tên `isDeveloper` trên đối tượng nguồn, và gán giá trị của nó cho một định danh có tên `isDev`. Nó giống như một sự "đổi tên" từ nguồn sang đích. Ngược lại, `firstName: firstName` đang cung cấp nguồn và đích cho một phép gán, nhưng là dư thừa vì chúng giống hệt nhau; một `firstName` duy nhất sẽ là đủ ở đây, và thường được ưa thích hơn.

`lastName: lname = "--thiếu--"` kết hợp cả đổi tên nguồn-đích và giá trị mặc định (nếu thuộc tính nguồn `lastName` bị thiếu hoặc `undefined`).

Đoạn mã trên kết hợp object destructuring với khai báo biến -- trong ví dụ này, `const` được sử dụng, nhưng `var` và `let` cũng hoạt động tốt -- nhưng nó không vốn là một cơ chế khai báo. Destructuring là về truy cập và gán (nguồn đến đích), vì vậy nó có thể hoạt động đối với các mục tiêu hiện có thay vì khai báo các mục tiêu mới:

```js
let fave;

// bao quanh ( ) là cú pháp bắt buộc ở đây,
// khi một bộ khai báo không được sử dụng
({ favoriteNumber: fave } = myObj);

fave;  // 42
```

Cú pháp object destructuring thường được ưa thích vì phong cách khai báo và dễ đọc hơn, so với các tương đương mệnh lệnh nặng nề trước ES6. Nhưng đừng lạm dụng destructuring. Đôi khi chỉ cần làm `x = someObj.x` là hoàn toàn ổn!

### Truy cập Thuộc tính Có điều kiện

Gần đây (trong ES2020), một tính năng được gọi là "optional chaining" (chuỗi tùy chọn) đã được thêm vào JS, giúp tăng cường khả năng truy cập thuộc tính (đặc biệt là truy cập thuộc tính lồng nhau). Dạng chính là toán tử ghép hai ký tự `?.`, như `A?.B`.

Toán tử này sẽ kiểm tra tham chiếu bên trái (`A`) xem nó có phải là null'ish (`null` hoặc `undefined`) hay không. Nếu có, phần còn lại của biểu thức truy cập thuộc tính bị ngắn mạch (bỏ qua), và `undefined` được trả về dưới dạng kết quả (ngay cả khi `null` thực sự gặp phải!). Nếu không, `?.` sẽ truy cập thuộc tính giống như một toán tử `.` bình thường.

Ví dụ:

```js
myObj?.favoriteNumber
```

Ở đây, kiểm tra null'ish được thực hiện đối với `myObj`, nghĩa là truy cập thuộc tính `favoriteNumber` chỉ được thực hiện nếu giá trị trong `myObj` là không null'ish. Lưu ý rằng nó không xác minh rằng `myObj` thực sự đang giữ một đối tượng thực sự, chỉ là nó không nullish. Tuy nhiên, tất cả các giá trị không nullish đều có thể được "truy cập" một cách "an toàn" (không có ngoại lệ JS) thông qua toán tử `.`, ngay cả khi không có thuộc tính phù hợp nào để truy xuất.

Rất dễ bị nhầm lẫn khi nghĩ rằng kiểm tra null'ish là đối với thuộc tính `favoriteNumber`. Nhưng một cách để giữ cho nó thẳng thắn là nhớ rằng `?` ở phía kiểm tra an toàn được thực hiện, trong khi `.` ở phía chỉ được đánh giá có điều kiện nếu kiểm tra không null'ish vượt qua.

Thông thường, toán tử `?.` được sử dụng trong các truy cập thuộc tính lồng nhau có thể sâu 3 cấp trở lên, chẳng hạn như:

```js
myObj?.address?.city
```

Hoạt động tương đương với toán tử `?.` sẽ trông như thế này:

```js
(myObj != null && myObj.address != null) ? myObj.address.city : undefined
```

Một lần nữa, hãy nhớ rằng không có kiểm tra nào được thực hiện đối với thuộc tính ngoài cùng bên phải (`city`) ở đây.

Ngoài ra, `?.` không nên được sử dụng phổ biến thay cho mọi toán tử `.` trong các chương trình của bạn. Bạn nên cố gắng biết liệu truy cập thuộc tính `.` có thành công hay không trước khi thực hiện truy cập, bất cứ khi nào có thể. Chỉ sử dụng `?.` khi bản chất của các giá trị đang được truy cập chịu sự chi phối của các điều kiện không thể dự đoán/kiểm soát.

Ví dụ, trong đoạn mã trước, việc sử dụng `myObj?.` có lẽ là sai lầm, bởi vì thực sự không nên xảy ra trường hợp bạn bắt đầu một chuỗi truy cập thuộc tính đối với một biến thậm chí có thể không giữ một đối tượng cấp cao nhất (ngoài việc nội dung của nó có khả năng thiếu các thuộc tính nhất định trong các điều kiện nhất định).

Thay vào đó, tôi khuyên bạn nên sử dụng nhiều hơn như thế này:

```js
myObj.address?.city
```

Và biểu thức đó chỉ nên được sử dụng trong một phần của chương trình của bạn, nơi bạn chắc chắn rằng `myObj` ít nhất đang giữ một đối tượng hợp lệ (cho dù nó có thuộc tính `address` với một đối tượng con trong đó hay không).

Một dạng khác của toán tử "optional chaining" là `?.[`, được sử dụng khi truy cập thuộc tính bạn muốn thực hiện có điều kiện/an toàn yêu cầu dấu ngoặc `[ .. ]`.

```js
myObj["2 nicknames"]?.[0];   // "getify"
```

Mọi thứ được khẳng định về cách `?.` hành xử đều giống nhau đối với `?.[`.

| CẢNH BÁO: |
| :--- |
| Có một dạng thứ ba của tính năng này, được đặt tên là "optional call" (cuộc gọi tùy chọn), sử dụng `?.(` làm toán tử. Nó được sử dụng để thực hiện kiểm tra không null'ish trên một thuộc tính trước khi thực thi giá trị hàm trong thuộc tính. Ví dụ, thay vì `myObj.someFunc(42)`, bạn có thể làm `myObj.someFunc?.(42)`. `?.(` kiểm tra để đảm bảo `myObj.someFunc` là không null'ish trước khi gọi nó (với phần `(42)`). Mặc dù điều đó nghe có vẻ như một tính năng hữu ích, tôi nghĩ điều này đủ nguy hiểm để đảm bảo tránh hoàn toàn dạng/cấu trúc này.<br><br>Mối quan tâm của tôi là `?.(` làm cho nó có vẻ như chúng ta đang đảm bảo rằng hàm là "có thể gọi được" trước khi gọi nó, trong khi thực tế chúng ta chỉ kiểm tra xem nó có phải là không null'ish hay không. Không giống như `?.` có thể cho phép truy cập `.` "an toàn" đối với một giá trị không null'ish cũng không phải là một đối tượng, kiểm tra không null'ish `?.(` không "an toàn" tương tự. Nếu thuộc tính được đề cập có bất kỳ giá trị không null'ish, không phải hàm nào trong đó, như `true` hoặc `"Hello"`, phần gọi `(42)` sẽ được gọi và ném ra một ngoại lệ JS. Vì vậy, nói cách khác, dạng này thật không may đang ngụy trang là "an toàn" hơn thực tế, và do đó nên tránh trong hầu hết mọi trường hợp. Nếu một giá trị thuộc tính có thể *không phải là* một hàm, hãy thực hiện kiểm tra đầy đủ hơn cho tính chất hàm của nó trước khi cố gắng gọi nó. Đừng giả vờ rằng `?.(` đang làm điều đó cho bạn, hoặc những người đọc/bảo trì mã của bạn trong tương lai (bao gồm cả bản thân bạn trong tương lai!) có thể sẽ hối tiếc. |

### Truy cập Thuộc tính Trên Không phải Đối tượng

Điều này nghe có vẻ phản trực giác, nhưng bạn thường có thể truy cập các thuộc tính/phương thức từ các giá trị không phải là chính các đối tượng:

```js
fave = 42;

fave;              // 42
fave.toString();   // "42"
```

Ở đây, `fave` giữ một giá trị số `42` nguyên thủy. Vì vậy, làm thế nào chúng ta có thể thực hiện `.toString` để truy cập một thuộc tính từ nó, và sau đó `()` để gọi hàm được giữ trong thuộc tính đó?

Đây là một chủ đề sâu sắc hơn rất nhiều so với những gì chúng ta sẽ đi vào trong cuốn sách này; xem cuốn 4, "Kiểu & Ngữ pháp", của bộ này để biết thêm. Tuy nhiên, như một cái nhìn nhanh: nếu bạn thực hiện truy cập thuộc tính (`.` hoặc `[ .. ]`) đối với một giá trị không phải đối tượng, không null'ish, JS sẽ theo mặc định (tạm thời!) ép kiểu giá trị thành một biểu diễn bọc đối tượng, cho phép truy cập thuộc tính đối với đối tượng được khởi tạo ngầm đó.

Quá trình này thường được gọi là "boxing" (đóng hộp), như trong việc đặt một giá trị bên trong một "hộp" (thùng chứa đối tượng).

Vì vậy, trong đoạn mã trên, chỉ trong khoảnh khắc `.toString` đang được truy cập trên giá trị `42`, JS sẽ đóng hộp giá trị này thành một đối tượng `Number`, và sau đó thực hiện truy cập thuộc tính.

Lưu ý rằng `null` và `undefined` có thể được đối tượng hóa, bằng cách gọi `Object(null)` / `Object(undefined)`. Tuy nhiên, JS không tự động đóng hộp các giá trị null'ish này, vì vậy truy cập thuộc tính đối với chúng sẽ thất bại (như đã thảo luận trước đó trong phần "Truy cập Thuộc tính Có điều kiện").

| LƯU Ý: |
| :--- |
| Boxing có một đối tác: unboxing (mở hộp). Ví dụ, công cụ JS sẽ lấy một trình bao bọc đối tượng -- giống như một đối tượng `Number` được bọc quanh `42` -- được tạo bằng `Number(42)` hoặc `Object(42)` -- và mở nó để truy xuất `42` nguyên thủy cơ bản, bất cứ khi nào một phép toán (như `*` hoặc `-`) gặp phải một đối tượng như vậy. Hành vi unboxing nằm ngoài phạm vi thảo luận của chúng tôi, nhưng được đề cập đầy đủ trong tiêu đề "Kiểu & Ngữ pháp" đã nói ở trên. |

## Gán Thuộc tính

Cho dù một thuộc tính được định nghĩa tại thời điểm định nghĩa object literal, hay được thêm vào sau đó, việc gán giá trị thuộc tính được thực hiện với toán tử `=`, như bất kỳ phép gán bình thường nào khác:

```js
myObj.favoriteNumber = 123;
```

Nếu thuộc tính `favoriteNumber` chưa tồn tại, câu lệnh đó sẽ tạo một thuộc tính mới có tên đó và gán giá trị của nó. Nhưng nếu nó đã tồn tại, câu lệnh đó sẽ gán lại giá trị của nó.

| CẢNH BÁO: |
| :--- |
| Một phép gán `=` cho một thuộc tính có thể thất bại (âm thầm hoặc ném ra ngoại lệ), hoặc nó có thể không trực tiếp gán giá trị mà thay vào đó gọi một hàm *setter* thực hiện một số thao tác. Thêm chi tiết về các hành vi này trong chương tiếp theo. |

Cũng có thể gán một hoặc nhiều thuộc tính cùng một lúc -- giả sử các thuộc tính nguồn (cặp tên và giá trị) nằm trong một đối tượng khác -- sử dụng phương thức `Object.assign(..)` (được thêm vào trong ES6):

```js
// sao chép nông tất cả các thuộc tính (được sở hữu và có thể liệt kê)
// từ `myObj` vào `anotherObj`
Object.assign(anotherObj,myObj);

Object.assign(
    /*đích=*/anotherObj,
    /*nguồn1=*/{
        someProp: "một số giá trị",
        anotherProp: 1001,
    },
    /*nguồn2=*/{
        yetAnotherProp: false
    }
);
```

`Object.assign(..)` lấy đối tượng đầu tiên làm đích, và (các) đối tượng thứ hai (và tùy chọn tiếp theo) làm nguồn. Việc sao chép được thực hiện theo cách tương tự như được mô tả trước đó trong phần "Object Spread".

## Xóa Thuộc tính

Khi một thuộc tính được định nghĩa trên một đối tượng, cách duy nhất để loại bỏ nó là với toán tử `delete`:

```js
anotherObj = {
    counter: 123
};

anotherObj.counter;   // 123

delete anotherObj.counter;

anotherObj.counter;   // undefined
```

Trái ngược với quan niệm sai lầm phổ biến, toán tử `delete` của JS **không** trực tiếp thực hiện bất kỳ việc giải phóng/giải phóng bộ nhớ nào, thông qua thu gom rác (GC). Điều duy nhất nó làm là loại bỏ một thuộc tính khỏi một đối tượng. Nếu giá trị trong thuộc tính là một tham chiếu (đến một đối tượng khác/v.v.), và không có tham chiếu nào khác còn tồn tại đến giá trị đó sau khi thuộc tính bị loại bỏ, giá trị đó có thể sẽ đủ điều kiện để loại bỏ trong một lần quét GC trong tương lai.

Gọi `delete` trên bất kỳ thứ gì khác ngoài thuộc tính đối tượng là lạm dụng toán tử `delete`, và sẽ thất bại âm thầm (trong chế độ không nghiêm ngặt) hoặc ném ra ngoại lệ (trong chế độ nghiêm ngặt).

Xóa một thuộc tính khỏi một đối tượng khác biệt với việc gán cho nó một giá trị như `undefined` hoặc `null`. Một thuộc tính được gán `undefined`, ban đầu hoặc sau đó, vẫn hiện diện trên đối tượng, và vẫn có thể được tiết lộ khi liệt kê nội dung.

## Xác định Nội dung Thùng chứa

Bạn có thể xác định nội dung của một đối tượng theo nhiều cách. Để hỏi một đối tượng xem nó có một thuộc tính cụ thể hay không:

```js
myObj = {
    favoriteNumber: 42,
    coolFact: "người đầu tiên bị kết tội chạy quá tốc độ đã đi với tốc độ 8 dặm/giờ",
    beardLength: undefined,
    nicknames: [ "getify", "ydkjs" ]
};

"favoriteNumber" in myObj;            // true

myObj.hasOwnProperty("coolFact");     // true
myObj.hasOwnProperty("beardLength");  // true

myObj.nicknames = undefined;
myObj.hasOwnProperty("nicknames");    // true

delete myObj.nicknames;
myObj.hasOwnProperty("nicknames");    // false
```

*Có* một sự khác biệt quan trọng giữa cách toán tử `in` và phương thức `hasOwnProperty(..)` hành xử. Toán tử `in` sẽ kiểm tra không chỉ đối tượng đích được chỉ định, mà nếu không tìm thấy ở đó, nó cũng sẽ tham khảo chuỗi `[[Prototype]]` của đối tượng (được đề cập trong chương tiếp theo). Ngược lại, `hasOwnProperty(..)` chỉ tham khảo đối tượng đích.

Nếu bạn chú ý kỹ, bạn có thể nhận thấy rằng `myObj` dường như có một thuộc tính phương thức gọi là `hasOwnProperty(..)` trên đó, mặc dù chúng ta không định nghĩa như vậy. Đó là bởi vì `hasOwnProperty(..)` được định nghĩa như một tích hợp trên `Object.prototype`, theo mặc định được "kế thừa bởi" tất cả các đối tượng bình thường. Tuy nhiên, có rủi ro vốn có khi truy cập một phương thức "được kế thừa" như vậy. Một lần nữa, thêm về các prototype trong chương tiếp theo.

### Kiểm tra Sự tồn tại Tốt hơn

ES2022 (gần như chính thức tại thời điểm viết) đã giải quyết một tính năng mới, `Object.hasOwn(..)`. Nó thực hiện về cơ bản điều tương tự như `hasOwnProperty(..)`, nhưng nó được gọi như một trình trợ giúp tĩnh bên ngoài giá trị đối tượng thay vì thông qua `[[Prototype]]` của đối tượng, làm cho nó an toàn hơn và nhất quán hơn trong việc sử dụng:

```js
// thay vì:
myObj.hasOwnProperty("favoriteNumber")

// bây giờ chúng ta nên ưu tiên:
Object.hasOwn(myObj,"favoriteNumber")
```

Mặc dù (tại thời điểm viết) tính năng này chỉ mới xuất hiện trong JS, có các polyfill làm cho API này có sẵn trong các chương trình của bạn ngay cả khi chạy trong môi trường JS trước đó chưa có tính năng được định nghĩa. Ví dụ, một bản phác thảo polyfill thay thế nhanh chóng:

```js
// phác thảo polyfill đơn giản cho `Object.hasOwn(..)`
if (!Object.hasOwn) {
    Object.hasOwn = function hasOwn(obj,propName) {
        return Object.prototype.hasOwnProperty.call(obj,propName);
    };
}
```

Bao gồm một bản vá polyfill như vậy trong chương trình của bạn có nghĩa là bạn có thể bắt đầu sử dụng `Object.hasOwn(..)` một cách an toàn để kiểm tra sự tồn tại của thuộc tính bất kể môi trường JS có `Object.hasOwn(..)` được tích hợp sẵn hay chưa.

### Liệt kê Tất cả Nội dung Thùng chứa

Chúng ta đã thảo luận về API `Object.entries(..)` trước đó, cho chúng ta biết một đối tượng có những thuộc tính nào (miễn là chúng có thể liệt kê -- thêm trong chương tiếp theo).

Cũng có nhiều cơ chế khác có sẵn. `Object.keys(..)` cung cấp cho chúng ta danh sách các tên thuộc tính có thể liệt kê (hay còn gọi là khóa) trong một đối tượng -- chỉ tên, không có giá trị; `Object.values(..)` thay vào đó cung cấp cho chúng ta danh sách tất cả các giá trị được giữ trong các thuộc tính có thể liệt kê.

Nhưng nếu chúng ta muốn lấy *tất cả* các khóa trong một đối tượng (có thể liệt kê hay không)? `Object.getOwnPropertyNames(..)` dường như làm những gì chúng ta muốn, ở chỗ nó giống như `Object.keys(..)` nhưng cũng trả về các tên thuộc tính không thể liệt kê. Tuy nhiên, danh sách này **sẽ không** bao gồm bất kỳ tên thuộc tính Symbol nào, vì chúng được coi là các vị trí đặc biệt trên đối tượng. `Object.getOwnPropertySymbols(..)` trả về tất cả các thuộc tính Symbol của một đối tượng. Vì vậy, nếu bạn nối cả hai danh sách đó lại với nhau, bạn sẽ có tất cả nội dung trực tiếp (*được sở hữu*) của một đối tượng.

Tuy nhiên, như chúng ta đã ngụ ý nhiều lần, và sẽ đề cập đầy đủ chi tiết trong chương tiếp theo, một đối tượng cũng có thể "kế thừa" nội dung từ chuỗi `[[Prototype]]` của nó. Những thứ này không được coi là nội dung *được sở hữu*, vì vậy chúng sẽ không hiển thị trong bất kỳ danh sách nào trong số này.

Hãy nhớ lại rằng toán tử `in` có khả năng sẽ duyệt qua toàn bộ chuỗi tìm kiếm sự tồn tại của một thuộc tính. Tương tự, một vòng lặp `for..in` sẽ duyệt qua chuỗi và liệt kê bất kỳ thuộc tính có thể liệt kê nào (được sở hữu hoặc được kế thừa). Nhưng không có API tích hợp nào sẽ duyệt qua toàn bộ chuỗi và trả về danh sách tập hợp kết hợp của cả nội dung *được sở hữu* và *được kế thừa*.

## Thùng chứa Tạm thời

Sử dụng một thùng chứa để giữ nhiều giá trị đôi khi chỉ là một cơ chế vận chuyển tạm thời, chẳng hạn như khi bạn muốn truyền nhiều giá trị cho một hàm thông qua một đối số duy nhất, hoặc khi bạn muốn một hàm trả về nhiều giá trị:

```js
function formatValues({ one, two, three }) {
    // đối tượng thực tế được truyền vào như một
    // đối số không thể truy cập được, vì
    // chúng ta đã destructure nó thành ba
    // biến riêng biệt

    one = one.toUpperCase();
    two = `--${two}--`;
    three = three.substring(0,5);

    // đối tượng này chỉ để vận chuyển
    // tất cả ba giá trị trong một
    // câu lệnh return duy nhất
    return { one, two, three };
}

// destructuring giá trị trả về từ
// hàm, vì đối tượng được trả về đó
// chỉ là một thùng chứa tạm thời
// để vận chuyển cho chúng ta nhiều giá trị
const { one, two, three } =

    // đối số đối tượng này là một phương tiện
    // vận chuyển tạm thời cho nhiều giá trị đầu vào
    formatValues({
       one: "Kyle",
       two: "Simpson",
       three: "getify"
    });

one;     // "KYLE"
two;     // "--Simpson--"
three;   // "getif"
```

Object literal được truyền vào `formatValues(..)` ngay lập tức được destructure tham số, vì vậy bên trong hàm chúng ta chỉ xử lý ba biến riêng biệt (`one`, `two`, và `three`). Object literal được `return` từ hàm cũng ngay lập tức được destructure, vì vậy một lần nữa chúng ta chỉ xử lý ba biến riêng biệt (`one`, `two`, `three`).

Đoạn mã này minh họa thành ngữ/mẫu rằng một đối tượng đôi khi chỉ là một thùng chứa vận chuyển tạm thời thay vì một giá trị có ý nghĩa trong và của chính nó.

## Thùng chứa là Tập hợp các Thuộc tính

Cách sử dụng phổ biến nhất của các đối tượng là làm thùng chứa cho nhiều giá trị. Chúng ta tạo và quản lý các đối tượng thùng chứa thuộc tính bằng cách:

* định nghĩa các thuộc tính (các vị trí được đặt tên), hoặc tại thời điểm tạo đối tượng hoặc sau đó
* gán giá trị, hoặc tại thời điểm tạo đối tượng hoặc sau đó
* truy cập giá trị sau đó, sử dụng tên vị trí (tên thuộc tính)
* xóa các thuộc tính thông qua `delete`
* xác định nội dung thùng chứa với `in`, `hasOwnProperty(..)` / `hasOwn(..)`, `Object.entries(..)` / `Object.keys(..)`, v.v.

Nhưng còn nhiều điều hơn đối với các đối tượng hơn chỉ là các tập hợp tĩnh của tên và giá trị thuộc tính. Trong chương tiếp theo, chúng ta sẽ đi sâu vào bên trong để xem cách chúng thực sự hoạt động.

[^structuredClone]: "Structured Clone Algorithm", HTML Specification; https://html.spec.whatwg.org/multipage/structured-data.html#structured-cloning ; Truy cập tháng 7 năm 2022
