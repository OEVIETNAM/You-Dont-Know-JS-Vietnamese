# You Don't Know JS Yet: Đối tượng & Lớp - Ấn bản thứ 2
# Chương 2: Cách Đối tượng Hoạt động

| LƯU Ý: |
| :--- |
| Đang trong quá trình thực hiện |

Các đối tượng không chỉ là thùng chứa cho nhiều giá trị, mặc dù rõ ràng đó là bối cảnh cho hầu hết các tương tác với các đối tượng.

Để hiểu đầy đủ cơ chế đối tượng trong JS, và tận dụng tối đa việc sử dụng các đối tượng trong các chương trình của chúng ta, chúng ta cần xem xét kỹ hơn một số đặc điểm của các đối tượng (và các thuộc tính của chúng) có thể ảnh hưởng đến hành vi của chúng khi tương tác với chúng.

Những đặc điểm xác định hành vi cơ bản của các đối tượng được gọi chung theo thuật ngữ chính thức là "giao thức metaobject" (MOP)[^mop]. MOP hữu ích không chỉ để hiểu cách các đối tượng sẽ hành xử, mà còn để ghi đè các hành vi mặc định của các đối tượng nhằm uốn nắn ngôn ngữ để phù hợp hơn với nhu cầu của chương trình của chúng ta.

## Mô tả Thuộc tính (Property Descriptors)

Mỗi thuộc tính trên một đối tượng được mô tả nội bộ bởi cái được gọi là "mô tả thuộc tính" (property descriptor). Bản thân nó là một đối tượng (hay còn gọi là "metaobject") với một vài thuộc tính (hay còn gọi là "thuộc tính") trên đó, quy định cách thuộc tính đích hành xử.

Chúng ta có thể truy xuất một mô tả thuộc tính cho bất kỳ thuộc tính hiện có nào bằng cách sử dụng `Object.getOwnPropertyDescriptor(..)` (ES5):

```js
myObj = {
    favoriteNumber: 42,
    isDeveloper: true,
    firstName: "Kyle"
};

Object.getOwnPropertyDescriptor(myObj,"favoriteNumber");
// {
//     value: 42,
//     enumerable: true,
//     writable: true,
//     configurable: true
// }
```

Chúng ta thậm chí có thể sử dụng một mô tả như vậy để định nghĩa một thuộc tính mới trên một đối tượng, bằng cách sử dụng `Object.defineProperty(..)` (ES5):

```js
anotherObj = {};

Object.defineProperty(anotherObj,"fave",{
    value: 42,
    enumerable: true,     // mặc định nếu bỏ qua
    writable: true,       // mặc định nếu bỏ qua
    configurable: true    // mặc định nếu bỏ qua
});

anotherObj.fave;          // 42
```

Nếu một thuộc tính hiện có chưa được đánh dấu là không thể cấu hình (với `configurable: false` trong mô tả của nó), nó luôn có thể được định nghĩa lại/ghi đè bằng cách sử dụng `Object.defineProperty(..)`.

| CẢNH BÁO: |
| :--- |
| Một số phần trước trong chương này đề cập đến việc "sao chép" hoặc "nhân bản" các thuộc tính. Người ta có thể cho rằng việc sao chép/nhân bản như vậy sẽ ở cấp độ mô tả thuộc tính. Tuy nhiên, không có hoạt động nào trong số đó thực sự hoạt động theo cách đó; tất cả chúng đều thực hiện truy cập và gán kiểu `=` đơn giản, có tác dụng bỏ qua bất kỳ sắc thái nào trong cách mô tả cơ bản cho một thuộc tính được định nghĩa. |

Mặc dù có vẻ ít phổ biến hơn nhiều trong thực tế, chúng ta thậm chí có thể định nghĩa nhiều thuộc tính cùng một lúc, mỗi thuộc tính có mô tả riêng:

```js
anotherObj = {};

Object.defineProperties(anotherObj,{
    "fave": {
        // một mô tả thuộc tính
    },
    "superFave": {
        // một mô tả thuộc tính khác
    }
});
```

Không phổ biến lắm khi thấy cách sử dụng này, bởi vì hiếm khi bạn cần kiểm soát cụ thể định nghĩa của nhiều thuộc tính. Nhưng nó có thể hữu ích trong một số trường hợp.

### Thuộc tính Accessor (Accessor Properties)

Một mô tả thuộc tính thường định nghĩa một thuộc tính `value`, như được hiển thị ở trên. Tuy nhiên, một loại thuộc tính đặc biệt, được gọi là "thuộc tính accessor" (hay còn gọi là getter/setter), có thể được định nghĩa. Đối với một thuộc tính như thế này, mô tả của nó không định nghĩa một thuộc tính `value` cố định, mà thay vào đó sẽ trông giống như thế này:

```js
{
    get() { .. },    // hàm để gọi khi truy xuất giá trị
    set(v) { .. },   // hàm để gọi khi gán giá trị
    // .. enumerable, v.v.
}
```

Một getter trông giống như một truy cập thuộc tính (`obj.prop`), nhưng bên dưới lớp vỏ nó gọi phương thức `get()` như đã định nghĩa; nó giống như thể bạn đã gọi `obj.prop()`. Một setter trông giống như một phép gán thuộc tính (`obj.prop = value`), nhưng nó gọi phương thức `set(..)` như đã định nghĩa; nó giống như thể bạn đã gọi `obj.prop(value)`.

Hãy minh họa một thuộc tính accessor getter/setter:

```js
anotherObj = {};

Object.defineProperty(anotherObj,"fave",{
    get() { console.log("Đang lấy giá trị 'fave'!"); return 123; },
    set(v) { console.log(`Đang bỏ qua phép gán ${v}.`); }
});

anotherObj.fave;
// Đang lấy giá trị 'fave'!
// 123

anotherObj.fave = 42;
// Đang bỏ qua phép gán 42.

anotherObj.fave;
// Đang lấy giá trị 'fave'!
// 123
```

### Enumerable, Writable, Configurable

Bên cạnh `value` hoặc `get()` / `set(..)`, 3 thuộc tính khác của một mô tả thuộc tính là (như được hiển thị ở trên):

* `enumerable`
* `writable`
* `configurable`

Thuộc tính `enumerable` kiểm soát xem thuộc tính có xuất hiện trong các liệt kê khác nhau của các thuộc tính đối tượng hay không, chẳng hạn như `Object.keys(..)`, `Object.entries(..)`, vòng lặp `for..in`, và việc sao chép xảy ra với `...` object spread và `Object.assign(..)`. Hầu hết các thuộc tính nên được để là có thể liệt kê (enumerable), nhưng bạn có thể đánh dấu một số thuộc tính đặc biệt nhất định trên một đối tượng là không thể liệt kê nếu chúng không nên được lặp lại/sao chép.

Thuộc tính `writable` kiểm soát xem một phép gán `value` (thông qua `=`) có được phép hay không. Để làm cho một thuộc tính "chỉ đọc", hãy định nghĩa nó với `writable: false`. Tuy nhiên, miễn là thuộc tính vẫn có thể cấu hình (configurable), `Object.defineProperty(..)` vẫn có thể thay đổi giá trị bằng cách đặt `value` khác đi.

Thuộc tính `configurable` kiểm soát xem **mô tả** của một thuộc tính có thể được định nghĩa lại/ghi đè hay không. Một thuộc tính `configurable: false` bị khóa với định nghĩa của nó, và bất kỳ nỗ lực nào tiếp theo để thay đổi nó với `Object.defineProperty(..)` sẽ thất bại. Một thuộc tính không thể cấu hình vẫn có thể được gán các giá trị mới (thông qua `=`), miễn là `writable: true` vẫn được đặt trên mô tả của thuộc tính.

## Kiểu con Đối tượng (Object Sub-Types)

Có nhiều loại kiểu con chuyên biệt của các đối tượng trong JS. Nhưng cho đến nay, hai loại phổ biến nhất bạn sẽ tương tác là mảng và `function` (hàm).

| LƯU Ý: |
| :--- |
| Bằng "kiểu con", chúng tôi muốn nói đến khái niệm về một kiểu dẫn xuất đã kế thừa các hành vi từ một kiểu cha nhưng sau đó chuyên biệt hóa hoặc mở rộng các hành vi đó. Nói cách khác, các giá trị của các kiểu con này hoàn toàn là các đối tượng, nhưng cũng *nhiều hơn chỉ là* các đối tượng. |

### Mảng (Arrays)

Mảng là các đối tượng được thiết kế đặc biệt để được **lập chỉ mục bằng số**, thay vì sử dụng các vị trí thuộc tính được đặt tên bằng chuỗi. Chúng vẫn là các đối tượng, vì vậy một thuộc tính được đặt tên như `favoriteNumber` là hợp lệ. Nhưng việc trộn lẫn các thuộc tính được đặt tên vào các mảng được lập chỉ mục bằng số rất không được khuyến khích.

Mảng tốt nhất là được định nghĩa với cú pháp literal (tương tự như các đối tượng), nhưng với các dấu ngoặc vuông `[ .. ]` thay vì các dấu ngoặc nhọn `{ .. }`:

```js
myList = [ 23, 42, 109 ];
```

JS cho phép bất kỳ sự pha trộn nào của các loại giá trị trong mảng, bao gồm các đối tượng, các mảng khác, các hàm, v.v. Như bạn có thể đã biết, các mảng được "lập chỉ mục bắt đầu từ 0", nghĩa là phần tử đầu tiên trong mảng ở chỉ mục `0`, không phải `1`:

```js
myList = [ 23, 42, 109 ];

myList[0];      // 23
myList[1];      // 42
```

Hãy nhớ lại rằng bất kỳ tên thuộc tính chuỗi nào trên một đối tượng mà "trông giống như" một số nguyên -- có thể được ép kiểu hợp lệ thành một số nguyên -- thực sự sẽ được xử lý giống như một thuộc tính số nguyên (hay còn gọi là chỉ mục số nguyên). Điều tương tự cũng áp dụng cho các mảng. Bạn nên luôn sử dụng `42` làm chỉ mục số nguyên (hay còn gọi là tên thuộc tính), nhưng nếu bạn sử dụng chuỗi `"42"`, JS sẽ giả định bạn có ý đó là một số nguyên và thực hiện điều đó cho bạn.

```js
// "2" hoạt động như một chỉ mục số nguyên ở đây, nhưng không được khuyến khích
myList["2"];    // 109
```

Một ngoại lệ cho *quy tắc* "không có thuộc tính được đặt tên trên mảng" là tất cả các mảng tự động hiển thị một thuộc tính `length`, thuộc tính này được tự động cập nhật với "độ dài" của mảng.

```js
myList = [ 23, 42, 109 ];

myList.length;   // 3

// "đẩy" một giá trị khác vào cuối danh sách
myList.push("Hello");

myList.length;   // 4
```

| CẢNH BÁO: |
| :--- |
| Nhiều nhà phát triển JS tin tưởng sai lầm rằng `length` của mảng về cơ bản là một *getter* (xem "Thuộc tính Accessor" trước đó trong chương này), nhưng không phải vậy. Hệ quả là những nhà phát triển này cảm thấy như việc truy cập thuộc tính này là "đắt đỏ" -- như thể JS phải tính toán lại độ dài ngay lập tức -- và do đó sẽ làm những việc như nắm bắt/lưu trữ độ dài của một mảng trước khi thực hiện một vòng lặp không thay đổi trên nó. Điều này từng là "thực hành tốt nhất" từ góc độ hiệu suất. Nhưng trong ít nhất 10 năm nay, đó thực sự là một anti-pattern, bởi vì công cụ JS hiệu quả hơn trong việc quản lý thuộc tính `length` so với mã JS của chúng ta khi cố gắng "vượt mặt" công cụ để tránh gọi một cái gì đó mà chúng ta nghĩ là một *getter*. Hiệu quả hơn là để công cụ JS làm công việc của nó, và chỉ cần truy cập thuộc tính bất cứ khi nào và bao nhiêu lần cần thiết. |

#### Các Khe Rỗng (Empty Slots)

Các mảng JS cũng có một "khiếm khuyết" thực sự đáng tiếc trong thiết kế của chúng, được gọi là "các khe rỗng". Nếu bạn gán một chỉ mục của một mảng vượt quá một vị trí so với phần cuối hiện tại của mảng, JS sẽ để các khe ở giữa "rỗng" thay vì tự động gán chúng thành `undefined` như bạn có thể mong đợi:

```js
myList = [ 23, 42, 109 ];
myList.length;              // 3

myList[14] = "Hello";
myList.length;              // 15

myList;                     // [ 23, 42, 109, empty x 11, "Hello" ]

// trông giống như một khe thực sự với một
// giá trị `undefined` thực sự trong đó,
// nhưng hãy coi chừng, đó là một mẹo!
myList[9];                  // undefined
```

Bạn có thể tự hỏi tại sao các khe rỗng lại tệ đến vậy? Một lý do: có các API trong JS, giống như `map(..)` của mảng, nơi các khe rỗng bị bỏ qua một cách đáng ngạc nhiên! Đừng bao giờ, đừng bao giờ cố ý tạo các khe rỗng trong các mảng của bạn. Điều này không thể tranh cãi là một trong những "phần tồi tệ" của JS.

### Hàm (Functions)

Tôi không có nhiều điều cụ thể để nói về các hàm ở đây, ngoài việc chỉ ra rằng chúng cũng là các kiểu đối tượng con. Điều này có nghĩa là ngoài việc có thể thực thi, chúng cũng có thể có các thuộc tính được đặt tên được thêm vào hoặc truy cập từ chúng.

Các hàm có hai thuộc tính được định nghĩa trước mà bạn có thể thấy mình đang tương tác, cụ thể cho các mục đích lập trình meta:

```js
function help(opt1,opt2,...remainingOpts) {
    // ..
}

help.name;          // "help"
help.length;        // 2
```

`length` của một hàm là số lượng các tham số được định nghĩa rõ ràng của nó, lên đến nhưng không bao gồm một tham số có giá trị mặc định được định nghĩa (ví dụ: `param = 42`) hoặc một "tham số còn lại" (ví dụ: `...remainingOpts`).

#### Tránh Đặt Thuộc tính Hàm-Đối tượng

Bạn nên tránh gán các thuộc tính trên các đối tượng hàm. Nếu bạn đang tìm cách lưu trữ thông tin bổ sung liên quan đến một hàm, hãy sử dụng một `Map(..)` riêng biệt (hoặc `WeakMap(..)`) với đối tượng hàm làm khóa, và thông tin bổ sung làm giá trị.

```js
extraInfo = new Map();

extraInfo.set(help,"đây là một số thông tin quan trọng");

// sau đó:
extraInfo.get(help);   // "đây là một số thông tin quan trọng"
```

## Đặc điểm Đối tượng (Object Characteristics)

Ngoài việc định nghĩa các hành vi cho các thuộc tính cụ thể, một số hành vi nhất định có thể cấu hình trên toàn bộ đối tượng:

* extensible (có thể mở rộng)
* sealed (đã niêm phong)
* frozen (đã đóng băng)

### Extensible (Có thể mở rộng)

Khả năng mở rộng đề cập đến việc liệu một đối tượng có thể có các thuộc tính mới được định nghĩa/thêm vào nó hay không. Theo mặc định, tất cả các đối tượng đều có thể mở rộng, nhưng bạn có thể tắt khả năng mở rộng cho một đối tượng:

```js
myObj = {
    favoriteNumber: 42
};

myObj.firstName = "Kyle";                  // hoạt động tốt

Object.preventExtensions(myObj);

myObj.nicknames = [ "getify", "ydkjs" ];   // thất bại
myObj.favoriteNumber = 123;                // hoạt động tốt
```

Trong chế độ không nghiêm ngặt, một phép gán tạo ra một thuộc tính mới sẽ thất bại âm thầm, trong khi ở chế độ nghiêm ngặt, một ngoại lệ sẽ được ném ra.

### Sealed (Đã niêm phong)

// TODO

### Frozen (Đã đóng băng)

// TODO

## Mở rộng MOP

Như đã đề cập ở đầu chương này, các đối tượng trong JS hành xử theo một bộ quy tắc được gọi là Giao thức Metaobject (MOP)[^mop]. Bây giờ chúng ta đã hiểu đầy đủ hơn về cách các đối tượng hoạt động theo mặc định, chúng ta muốn chuyển sự chú ý sang cách chúng ta có thể móc nối vào một số hành vi mặc định này và ghi đè/tùy chỉnh chúng.

// TODO

## Chuỗi `[[Prototype]]`

Một trong những đặc điểm quan trọng nhất, nhưng ít rõ ràng nhất, của một đối tượng (một phần của MOP) được gọi là "chuỗi prototype" của nó; ký hiệu đặc tả JS chính thức là `[[Prototype]]`. Hãy chắc chắn không nhầm lẫn `[[Prototype]]` này với một thuộc tính công khai có tên là `prototype`. Mặc dù tên gọi giống nhau, đây là những khái niệm riêng biệt.

`[[Prototype]]` là một liên kết nội bộ mà một đối tượng nhận được theo mặc định khi nó được tạo, trỏ đến một đối tượng khác. Liên kết này là một đặc điểm ẩn, thường tinh tế của một đối tượng, nhưng nó có tác động sâu sắc đến cách các tương tác với đối tượng sẽ diễn ra. Nó được gọi là một "chuỗi" bởi vì một đối tượng liên kết đến một đối tượng khác, đối tượng đó lại liên kết đến một đối tượng khác, ... và cứ thế. Có một *điểm cuối* hoặc *đỉnh* của chuỗi này, nơi liên kết dừng lại và không còn nơi nào để đi tiếp. Thêm về điều đó ngay sau đây.

Chúng ta đã thấy một số ý nghĩa của liên kết `[[Prototype]]` trong Chương 1. Ví dụ, theo mặc định, tất cả các đối tượng đều được liên kết `[[Prototype]]` với đối tượng tích hợp có tên là `Object.prototype`.

| CẢNH BÁO: |
| :--- |
| Bản thân cái tên `Object.prototype` đó có thể gây nhầm lẫn, vì nó sử dụng một thuộc tính gọi là `prototype`. `[[Prototype]]` và `prototype` liên quan như thế nào!? Hãy tạm dừng những câu hỏi/sự nhầm lẫn như vậy một chút, vì chúng ta sẽ quay lại và giải thích sự khác biệt giữa `[[Prototype]]` và `prototype` sau trong chương này. Hiện tại, chỉ cần giả định sự hiện diện của đối tượng tích hợp quan trọng nhưng có tên kỳ lạ này, `Object.prototype`. |

Hãy xem xét một số mã:

```js
myObj = {
    favoriteNumber: 42
};
```

Điều đó trông quen thuộc từ Chương 1. Nhưng những gì bạn *không thấy* trong mã này là đối tượng ở đó đã được tự động liên kết (thông qua `[[Prototype]]` nội bộ của nó) với đối tượng `Object.prototype` được tích hợp tự động nhưng có tên kỳ lạ đó.

Khi chúng ta làm những việc như:

```js
myObj.toString();                             // "[object Object]"

myObj.hasOwnProperty("favoriteNumber");   // true
```

Chúng ta đang tận dụng liên kết `[[Prototype]]` nội bộ này mà không thực sự nhận ra nó. Vì `myObj` không có các thuộc tính `toString` hoặc `hasOwnProperty` được định nghĩa trên nó, các truy cập thuộc tính đó thực sự kết thúc bằng việc **ỦY QUYỀN** (DELEGATING) quyền truy cập để tiếp tục tra cứu dọc theo chuỗi `[[Prototype]]`.

Vì `myObj` được liên kết `[[Prototype]]` với đối tượng có tên `Object.prototype`, việc tra cứu các thuộc tính `toString` và `hasOwnProperty` tiếp tục trên đối tượng đó; và quả thực, các phương thức này được tìm thấy ở đó!

Khả năng `myObj.toString` truy cập thuộc tính `toString` mặc dù nó thực sự không có nó, thường được gọi là "kế thừa", hoặc cụ thể hơn là "kế thừa nguyên mẫu" (prototypal inheritance). Các thuộc tính `toString` và `hasOwnProperty`, cùng với nhiều thuộc tính khác, được cho là "các thuộc tính được kế thừa" trên `myObj`.

| LƯU Ý: |
| :--- |
| Tôi có rất nhiều thất vọng với việc sử dụng từ "kế thừa" ở đây -- nó nên được gọi là "ủy quyền"! -- nhưng đó là những gì hầu hết mọi người gọi nó, vì vậy chúng tôi sẽ miễn cưỡng tuân thủ và sử dụng cùng một thuật ngữ đó cho đến bây giờ (mặc dù phản đối, với dấu ngoặc kép "). Tôi sẽ dành sự phản đối của mình cho một phụ lục của cuốn sách này. |

`Object.prototype` có một số thuộc tính và phương thức tích hợp, tất cả đều được "kế thừa" bởi bất kỳ đối tượng nào được liên kết `[[Prototype]]`, trực tiếp hoặc gián tiếp thông qua liên kết của một đối tượng khác, với `Object.prototype`.

Một số thuộc tính "được kế thừa" phổ biến từ `Object.prototype` bao gồm:

* `constructor`
* `__proto__`
* `toString()`
* `valueOf()`
* `hasOwnProperty(..)`
* `isPrototypeOf(..)`

Hãy nhớ lại `hasOwnProperty(..)`, mà chúng ta đã thấy trước đó cung cấp cho chúng ta một kiểm tra boolean xem một thuộc tính nhất định (theo tên chuỗi) có được sở hữu bởi một đối tượng hay không:

```js
myObj = {
    favoriteNumber: 42
};

myObj.hasOwnProperty("favoriteNumber");   // true
```

Luôn được coi là hơi đáng tiếc (tổ chức ngữ nghĩa, xung đột đặt tên, v.v.) khi một tiện ích quan trọng như `hasOwnProperty(..)` được đưa vào chuỗi `[[Prototype]]` của Object như một phương thức thể hiện (instance method), thay vì được định nghĩa là một tiện ích tĩnh.

Kể từ ES2022, JS cuối cùng đã thêm phiên bản tĩnh của tiện ích này: `Object.hasOwn(..)`.

```js
myObj = {
    favoriteNumber: 42
};

Object.hasOwn(myObj,"favoriteNumber");   // true
```

Dạng này hiện được coi là tùy chọn thích hợp và mạnh mẽ hơn, và dạng phương thức thể hiện (`hasOwnProperty(..)`) bây giờ thường nên tránh.

Hơi đáng tiếc và không nhất quán, vẫn chưa có (tại thời điểm viết) các tiện ích tĩnh tương ứng, như `Object.isPrototype(..)` (thay vì phương thức thể hiện `isPrototypeOf(..)`). Nhưng ít nhất `Object.hasOwn(..)` tồn tại, vì vậy đó là sự tiến bộ.

### Tạo Một Đối tượng Với `[[Prototype]]` Khác

Theo mặc định, bất kỳ đối tượng nào bạn tạo trong các chương trình của mình sẽ được liên kết `[[Prototype]]` với đối tượng `Object.prototype` đó. Tuy nhiên, bạn có thể tạo một đối tượng với một liên kết khác như thế này:

```js
myObj = Object.create(differentObj);
```

Phương thức `Object.create(..)` lấy đối số đầu tiên của nó làm giá trị để đặt cho `[[Prototype]]` của đối tượng mới được tạo.

Một nhược điểm của cách tiếp cận này là bạn không sử dụng cú pháp literal `{ .. }`, vì vậy bạn không định nghĩa ban đầu bất kỳ nội dung nào cho `myObj`. Bạn thường phải định nghĩa các thuộc tính từng cái một, sử dụng `=`.

| LƯU Ý: |
| :--- |
| Đối số thứ hai, tùy chọn cho `Object.create(..)` là -- giống như đối số thứ hai cho `Object.defineProperties(..)` như đã thảo luận trước đó -- một đối tượng với các thuộc tính giữ các mô tả để định nghĩa ban đầu đối tượng mới. Trong thực tế, dạng này hiếm khi được sử dụng, có thể vì nó khó xử hơn khi chỉ định đầy đủ các mô tả thay vì chỉ các cặp tên/giá trị. Nhưng nó có thể hữu ích trong một số trường hợp hạn chế. |

Ngoài ra, nhưng ít được ưa thích hơn, bạn có thể sử dụng cú pháp literal `{ .. }` cùng với một thuộc tính đặc biệt (và trông lạ lùng!):

```js
myObj = {
    __proto__: differentObj,

    // .. phần còn lại của định nghĩa đối tượng
};
```

| CẢNH BÁO: |
| :--- |
| Thuộc tính `__proto__` trông lạ lùng đã có trong một số công cụ JS trong hơn 20 năm, nhưng chỉ được chuẩn hóa trong JS kể từ ES6 (năm 2015). Thậm chí như vậy, nó đã được thêm vào Phụ lục B của đặc tả[^specApB], liệt kê các tính năng mà TC39 miễn cưỡng bao gồm vì chúng tồn tại phổ biến trong các công cụ JS dựa trên trình duyệt khác nhau và do đó là một thực tế hiển nhiên ngay cả khi chúng không bắt nguồn từ TC39. Do đó, tính năng này được đặc tả "đảm bảo" tồn tại trong tất cả các công cụ JS dựa trên trình duyệt tuân thủ, nhưng không nhất thiết được đảm bảo hoạt động trong các công cụ JS độc lập khác. Node.js sử dụng công cụ JS (v8) từ trình duyệt Chrome, vì vậy Node.js nhận được `__proto__` theo mặc định/tình cờ. Hãy cẩn thận khi sử dụng `__proto__` để nhận thức được tất cả các môi trường công cụ JS mà mã của bạn sẽ chạy trong đó. |

Cho dù bạn sử dụng `Object.create(..)` hay `__proto__`, đối tượng được đề cập thường sẽ được liên kết `[[Prototype]]` với một đối tượng khác với `Object.prototype` mặc định.

#### Liên kết `[[Prototype]]` Rỗng

Chúng ta đã đề cập ở trên rằng chuỗi `[[Prototype]]` phải dừng ở đâu đó, để việc tra cứu không tiếp tục mãi mãi. `Object.prototype` thường là đỉnh/cuối của mọi chuỗi `[[Prototype]]`, vì `[[Prototype]]` của chính nó là `null`, và do đó không còn nơi nào khác để tiếp tục tìm kiếm.

Tuy nhiên, bạn cũng có thể định nghĩa các đối tượng với giá trị `null` riêng của chúng cho `[[Prototype]]`, chẳng hạn như:

```js
emptyObj = Object.create(null);
// hoặc: emptyObj = { __proto__: null }

emptyObj.toString;   // undefined
```

Có thể khá hữu ích khi tạo một đối tượng không có liên kết `[[Prototype]]` với `Object.prototype`. Ví dụ, như đã đề cập trong Chương 1, các cấu trúc `in` và `for..in` sẽ tham khảo chuỗi `[[Prototype]]` cho các thuộc tính được kế thừa. Nhưng điều này có thể không mong muốn, vì bạn có thể không muốn một cái gì đó như `"toString" in myObj` giải quyết thành công.

Hơn nữa, một đối tượng với một `[[Prototype]]` rỗng an toàn khỏi bất kỳ sự va chạm "kế thừa" ngẫu nhiên nào giữa tên thuộc tính của chính nó và những tên nó "kế thừa" từ nơi khác. Những loại đối tượng (hữu ích!) này đôi khi được gọi trong ngôn ngữ phổ biến là "đối tượng từ điển" (dictionary objects).

### `[[Prototype]]` so với `prototype`

Chú ý tên thuộc tính công khai `prototype` trong tên/vị trí của đối tượng đặc biệt này, `Object.prototype`? Tất cả chuyện đó là sao?

`Object` là hàm `Object(..)`; theo mặc định, tất cả các hàm (chúng cũng là các đối tượng!) đều có một thuộc tính `prototype` như vậy trên chúng, trỏ vào một đối tượng.

Và đây là nơi xung đột tên giữa `[[Prototype]]` và `prototype` thực sự cắn chúng ta. Thuộc tính `prototype` trên một hàm không định nghĩa bất kỳ liên kết nào mà bản thân hàm trải nghiệm. Thật vậy, các hàm (như các đối tượng) có liên kết `[[Prototype]]` nội bộ riêng của chúng ở một nơi khác -- thêm về điều đó trong giây lát.

Thay vào đó, thuộc tính `prototype` trên một hàm đề cập đến một đối tượng nên được *liên kết ĐẾN* bởi bất kỳ đối tượng nào khác được tạo khi gọi hàm đó với từ khóa `new`:

```js
myObj = {};

// về cơ bản giống như:
myObj = new Object();
```

Vì cú pháp object literal `{ .. }` về cơ bản giống như một lệnh gọi `new Object()`, đối tượng tích hợp được đặt tên/đặt tại `Object.prototype` được sử dụng làm giá trị `[[Prototype]]` nội bộ cho đối tượng mới mà chúng ta tạo và đặt tên là `myObj`.

Phù! Nói về một chủ đề trở nên khó hiểu hơn đáng kể chỉ vì sự chồng chéo tên giữa `[[Prototype]]` và `prototype`!

----

Nhưng bản thân các hàm (như các đối tượng!) liên kết đến đâu, theo kiểu `[[Prototype]]`? Chúng liên kết đến `Function.prototype`, lại là một đối tượng tích hợp khác, nằm tại thuộc tính `prototype` trên hàm `Function(..)`.

Nói cách khác, bạn có thể nghĩ về bản thân các hàm như đã được "tạo ra" bởi một lệnh gọi `new Function(..)`, và sau đó được liên kết `[[Prototype]]` với đối tượng `Function.prototype`. Đối tượng này chứa các thuộc tính/phương thức mà tất cả các hàm "kế thừa" theo mặc định, chẳng hạn như `toString()` (để tuần tự hóa chuỗi mã nguồn của một hàm) và `call(..)` / `apply(..)` / `bind(..)` (chúng ta sẽ giải thích những điều này sau trong cuốn sách này).

## Hành vi Đối tượng

Các thuộc tính trên các đối tượng được định nghĩa và kiểm soát nội bộ bởi một metaobject "mô tả", bao gồm các thuộc tính như `value` (giá trị hiện tại của thuộc tính) và `enumerable` (một boolean kiểm soát xem thuộc tính có được bao gồm trong các danh sách chỉ liệt kê các thuộc tính/tên thuộc tính hay không).

Cách đối tượng và các thuộc tính của chúng hoạt động trong JS được gọi là "giao thức metaobject" (MOP)[^mop]. Chúng ta có thể kiểm soát hành vi chính xác của các thuộc tính thông qua `Object.defineProperty(..)`, cũng như các hành vi trên toàn bộ đối tượng với `Object.freeze(..)`. Nhưng thậm chí mạnh mẽ hơn, chúng ta có thể móc nối vào và ghi đè một số hành vi mặc định nhất định trên các đối tượng bằng cách sử dụng các Symbol được định nghĩa trước đặc biệt.

Các prototype là các liên kết nội bộ giữa các đối tượng cho phép truy cập thuộc tính hoặc phương thức đối với một đối tượng -- nếu thuộc tính/phương thức được yêu cầu vắng mặt -- được xử lý bằng cách "ủy quyền" tra cứu truy cập đó cho một đối tượng khác. Khi việc ủy quyền liên quan đến một phương thức, ngữ cảnh cho phương thức chạy được chia sẻ từ đối tượng ban đầu đến đối tượng đích thông qua từ khóa `this`.

[^mop]: "Metaobject", Wikipedia; https://en.wikipedia.org/wiki/Metaobject ; Truy cập tháng 7 năm 2022.

[^specApB]: "Appendix B: Additional ECMAScript Features for Web Browsers", ECMAScript 2022 Language Specification; https://262.ecma-international.org/13.0/#sec-additional-ecmascript-features-for-web-browsers ; Truy cập tháng 7 năm 2022
