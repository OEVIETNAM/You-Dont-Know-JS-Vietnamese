---
layout: default
title: Chương 3
parent: Bắt đầu
nav_order: 4
---

# You Don't Know JS Yet: Bắt đầu - Ấn bản thứ 2
# Chương 3: Đào sâu vào Cội rễ của JS

Nếu bạn đã đọc Chương 1 và 2, và dành thời gian để tiêu hóa và ngấm dần, hy vọng bạn đang bắt đầu *hiểu* JS thêm một chút. Nếu bạn đã bỏ qua/đọc lướt chúng (đặc biệt là Chương 2), tôi khuyên bạn nên quay lại để dành thêm thời gian với tài liệu đó.

Trong Chương 2, chúng ta đã khảo sát cú pháp, các mẫu và hành vi ở mức độ cao. Trong chương này, sự chú ý của chúng ta chuyển sang một số đặc điểm gốc rễ cấp thấp hơn của JS làm nền tảng cho hầu như mọi dòng mã chúng ta viết.

Hãy lưu ý: chương này đào sâu hơn nhiều so với những gì bạn có thể quen nghĩ về một ngôn ngữ lập trình. Mục tiêu của tôi là giúp bạn đánh giá cao cốt lõi của cách JS hoạt động, điều gì làm cho nó hoạt động. Chương này sẽ bắt đầu trả lời một số câu hỏi "Tại sao?" có thể xuất hiện khi bạn khám phá JS. Tuy nhiên, tài liệu này vẫn không phải là một sự trình bày đầy đủ về ngôn ngữ; đó là những gì phần còn lại của bộ sách dành cho! Mục tiêu của chúng ta ở đây vẫn chỉ là *bắt đầu*, và trở nên thoải mái hơn với, *cảm giác* của JS, cách nó thăng trầm.

Đừng chạy quá nhanh qua tài liệu này đến nỗi bạn bị lạc trong đám cỏ dại. Như tôi đã nói cả tá lần rồi, **hãy dành thời gian của bạn**. Thậm chí như vậy, bạn có thể sẽ kết thúc chương này với những câu hỏi còn lại. Điều đó là ổn, bởi vì có cả một bộ sách phía trước bạn để tiếp tục khám phá!

## Lặp lại (Iteration)

Vì các chương trình về cơ bản được xây dựng để xử lý dữ liệu (và đưa ra quyết định dựa trên dữ liệu đó), các mẫu được sử dụng để bước qua dữ liệu có tác động lớn đến khả năng đọc của chương trình.

Mẫu iterator (trình lặp) đã tồn tại trong nhiều thập kỷ, và gợi ý một cách tiếp cận "tiêu chuẩn hóa" để tiêu thụ dữ liệu từ một nguồn một *phần* tại một thời điểm. Ý tưởng là việc lặp lại nguồn dữ liệu—để xử lý dần dần tập hợp dữ liệu bằng cách xử lý phần đầu tiên, sau đó là phần tiếp theo, v.v., thay vì xử lý toàn bộ tập hợp cùng một lúc—là phổ biến và hữu ích hơn.

Hãy tưởng tượng một cấu trúc dữ liệu đại diện cho một truy vấn `SELECT` cơ sở dữ liệu quan hệ, thường tổ chức kết quả thành các hàng. Nếu truy vấn này chỉ có một hoặc một vài hàng, bạn có thể xử lý toàn bộ tập hợp kết quả cùng một lúc, và gán mỗi hàng cho một biến cục bộ, và thực hiện bất kỳ thao tác nào trên dữ liệu đó là phù hợp.

Nhưng nếu truy vấn có 100 hoặc 1.000 (hoặc nhiều hơn!) hàng, bạn sẽ cần xử lý lặp lại để giải quyết dữ liệu này (thường là một vòng lặp).

Mẫu iterator định nghĩa một cấu trúc dữ liệu được gọi là "iterator" có tham chiếu đến một nguồn dữ liệu cơ bản (như các hàng kết quả truy vấn), hiển thị một phương thức như `next()`. Gọi `next()` trả về phần dữ liệu tiếp theo (tức là một "bản ghi" hoặc "hàng" từ một truy vấn cơ sở dữ liệu).

Bạn không phải lúc nào cũng biết có bao nhiêu phần dữ liệu mà bạn sẽ cần lặp qua, vì vậy mẫu thường chỉ ra sự hoàn thành bằng một số giá trị đặc biệt hoặc ngoại lệ khi bạn lặp qua toàn bộ tập hợp và *đi qua phần cuối*.

Tầm quan trọng của mẫu iterator là tuân thủ một cách *tiêu chuẩn* để xử lý dữ liệu lặp đi lặp lại, tạo ra mã sạch hơn và dễ hiểu hơn, trái ngược với việc mỗi cấu trúc/nguồn dữ liệu xác định cách tùy chỉnh riêng của nó để xử lý dữ liệu của nó.

Sau nhiều năm nỗ lực khác nhau của cộng đồng JS xung quanh các kỹ thuật lặp lại được thỏa thuận chung, ES6 đã tiêu chuẩn hóa một giao thức cụ thể cho mẫu iterator trực tiếp trong ngôn ngữ. Giao thức định nghĩa một phương thức `next()` có giá trị trả về là một đối tượng được gọi là *kết quả iterator*; đối tượng có các thuộc tính `value` và `done`, trong đó `done` là một boolean là `false` cho đến khi việc lặp lại trên nguồn dữ liệu cơ bản hoàn tất.

### Tiêu thụ Iterator

Với giao thức lặp lại ES6 tại chỗ, việc tiêu thụ một nguồn dữ liệu một giá trị tại một thời điểm là khả thi, kiểm tra sau mỗi cuộc gọi `next()` để `done` là `true` để dừng việc lặp lại. Nhưng cách tiếp cận này khá thủ công, vì vậy ES6 cũng bao gồm một số cơ chế (cú pháp và API) để tiêu thụ tiêu chuẩn các iterator này.

Một cơ chế như vậy là vòng lặp `for..of`:

```js
// cho một iterator của một số nguồn dữ liệu:
var it = /* .. */;

// lặp qua các kết quả của nó từng cái một
for (let val of it) {
    console.log(`Iterator value: ${ val }`);
}
// Iterator value: ..
// Iterator value: ..
// ..
```

| LƯU Ý: |
| :--- |
| Chúng tôi sẽ bỏ qua tương đương vòng lặp thủ công ở đây, nhưng nó chắc chắn kém dễ đọc hơn vòng lặp `for..of`! |

Một cơ chế khác thường được sử dụng để tiêu thụ iterator là toán tử `...`. Toán tử này thực sự có hai dạng đối xứng: *spread* (trải ra) và *rest* (hoặc *gather* (thu thập), như tôi thích hơn). Dạng *spread* là một người tiêu dùng iterator.

Để *spread* một iterator, bạn phải có *thứ gì đó* để spread nó vào. Có hai khả năng trong JS: một mảng hoặc một danh sách đối số cho một cuộc gọi hàm.

Một spread mảng:

```js
// spread một iterator vào một mảng,
// với mỗi giá trị được lặp lại chiếm
// một vị trí phần tử mảng.
var vals = [ ...it ];
```

Một spread cuộc gọi hàm:

```js
// spread một iterator vào một hàm,
// gọi với mỗi giá trị được lặp lại
// chiếm một vị trí đối số.
doSomethingUseful( ...it );
```

Trong cả hai trường hợp, dạng iterator-spread của `...` tuân theo giao thức tiêu thụ iterator (giống như vòng lặp `for..of`) để lấy tất cả các giá trị có sẵn từ một iterator và đặt (hay còn gọi là, spread) chúng vào ngữ cảnh nhận (mảng, danh sách đối số).

### Iterables

Giao thức tiêu thụ iterator được định nghĩa kỹ thuật để tiêu thụ *iterables*; một iterable là một giá trị có thể được lặp lại.

Giao thức tự động tạo một thể hiện iterator từ một iterable, và tiêu thụ *chỉ thể hiện iterator đó* cho đến khi hoàn thành. Điều này có nghĩa là một iterable duy nhất có thể được tiêu thụ nhiều lần; mỗi lần, một thể hiện iterator mới sẽ được tạo và sử dụng.

Vậy chúng ta tìm iterables ở đâu?

ES6 đã định nghĩa các loại cấu trúc dữ liệu/tập hợp cơ bản trong JS là iterables. Điều này bao gồm chuỗi, mảng, map, set, và những thứ khác.

Hãy xem xét:

```js
// một mảng là một iterable
var arr = [ 10, 20, 30 ];

for (let val of arr) {
    console.log(`Array value: ${ val }`);
}
// Array value: 10
// Array value: 20
// Array value: 30
```

Vì mảng là iterables, chúng ta có thể sao chép nông (shallow-copy) một mảng bằng cách sử dụng tiêu thụ iterator thông qua toán tử spread `...`:

```js
var arrCopy = [ ...arr ];
```

Chúng ta cũng có thể lặp lại các ký tự trong một chuỗi từng cái một:

```js
var greeting = "Hello world!";
var chars = [ ...greeting ];

chars;
// [ "H", "e", "l", "l", "o", " ",
//   "w", "o", "r", "l", "d", "!" ]
```

Một cấu trúc dữ liệu `Map` sử dụng các đối tượng làm khóa, liên kết một giá trị (của bất kỳ loại nào) với đối tượng đó. Map có một lần lặp mặc định khác so với những gì thấy ở đây, ở chỗ việc lặp lại không chỉ qua các giá trị của map mà thay vào đó là các *mục* (entries) của nó. Một *mục* là một tuple (mảng 2 phần tử) bao gồm cả khóa và giá trị.

Hãy xem xét:

```js
// cho hai phần tử DOM, `btn1` và `btn2`

var buttonNames = new Map();
buttonNames.set(btn1,"Button 1");
buttonNames.set(btn2,"Button 2");

for (let [btn,btnName] of buttonNames) {
    btn.addEventListener("click",function onClick(){
        console.log(`Clicked ${ btnName }`);
    });
}
```

Trong vòng lặp `for..of` qua lần lặp map mặc định, chúng ta sử dụng cú pháp `[btn,btnName]` (được gọi là "phân rã mảng" - array destructuring) để chia nhỏ mỗi tuple được tiêu thụ thành các cặp khóa/giá trị tương ứng (`btn1` / `"Button 1"` và `btn2` / `"Button 2"`).

Mỗi iterable tích hợp trong JS hiển thị một lần lặp mặc định, một lần lặp có khả năng phù hợp với trực giác của bạn. Nhưng bạn cũng có thể chọn một lần lặp cụ thể hơn nếu cần thiết. Ví dụ, nếu chúng ta chỉ muốn tiêu thụ các giá trị của map `buttonNames` ở trên, chúng ta có thể gọi `values()` để lấy một iterator chỉ có giá trị:

```js
for (let btnName of buttonNames.values()) {
    console.log(btnName);
}
// Button 1
// Button 2
```

Hoặc nếu chúng ta muốn chỉ mục *và* giá trị trong một lần lặp mảng, chúng ta có thể tạo một iterator mục với phương thức `entries()`:

```js
var arr = [ 10, 20, 30 ];

for (let [idx,val] of arr.entries()) {
    console.log(`[${ idx }]: ${ val }`);
}
// [0]: 10
// [1]: 20
// [2]: 30
```

Phần lớn, tất cả các iterable tích hợp trong JS đều có sẵn ba dạng iterator: chỉ khóa (`keys()`), chỉ giá trị (`values()`), và mục (`entries()`).

Ngoài việc chỉ sử dụng các iterable tích hợp, bạn cũng có thể đảm bảo các cấu trúc dữ liệu của riêng mình tuân thủ giao thức lặp lại; làm như vậy có nghĩa là bạn chọn tham gia vào khả năng tiêu thụ dữ liệu của mình bằng các vòng lặp `for..of` và toán tử `...`. "Tiêu chuẩn hóa" trên giao thức này có nghĩa là mã tổng thể dễ nhận biết và dễ đọc hơn.

| LƯU Ý: |
| :--- |
| Bạn có thể đã nhận thấy một sự thay đổi sắc thái xảy ra trong cuộc thảo luận này. Chúng ta bắt đầu bằng cách nói về việc tiêu thụ **iterators**, nhưng sau đó chuyển sang nói về việc lặp lại qua **iterables**. Giao thức tiêu thụ lặp lại mong đợi một *iterable*, nhưng lý do chúng ta có thể cung cấp một *iterator* trực tiếp là vì một iterator chỉ là một iterable của chính nó! Khi tạo một thể hiện iterator từ một iterator hiện có, chính iterator đó được trả về. |

## Closure

Có lẽ không nhận ra điều đó, hầu như mọi nhà phát triển JS đều đã sử dụng closure. Trên thực tế, closure là một trong những chức năng lập trình phổ biến nhất trên phần lớn các ngôn ngữ. Nó thậm chí có thể quan trọng để hiểu như các biến hoặc vòng lặp; đó là mức độ cơ bản của nó.

Tuy nhiên, nó cảm thấy hơi ẩn, gần như kỳ diệu. Và nó thường được nói đến trong các thuật ngữ rất trừu tượng hoặc rất không chính thức, điều này không giúp ích nhiều cho chúng ta trong việc xác định chính xác nó là gì.

Chúng ta cần có khả năng nhận ra nơi closure được sử dụng trong các chương trình, vì sự hiện diện hoặc thiếu closure đôi khi là nguyên nhân gây ra lỗi (hoặc thậm chí là nguyên nhân gây ra các vấn đề về hiệu suất).

Vì vậy, hãy định nghĩa closure theo một cách thực dụng và cụ thể:

> Closure là khi một hàm ghi nhớ và tiếp tục truy cập các biến từ bên ngoài phạm vi của nó, ngay cả khi hàm được thực thi trong một phạm vi khác.

Chúng ta thấy hai đặc điểm định nghĩa ở đây. Thứ nhất, closure là một phần của bản chất của một hàm. Các đối tượng không có closure, các hàm có. Thứ hai, để quan sát một closure, bạn phải thực thi một hàm trong một phạm vi khác với nơi hàm đó được định nghĩa ban đầu.

Hãy xem xét:

```js
function greeting(msg) {
    return function who(name) {
        console.log(`${ msg }, ${ name }!`);
    };
}

var hello = greeting("Hello");
var howdy = greeting("Howdy");

hello("Kyle");
// Hello, Kyle!

hello("Sarah");
// Hello, Sarah!

howdy("Grant");
// Howdy, Grant!
```

Đầu tiên, hàm bên ngoài `greeting(..)` được thực thi, tạo ra một thể hiện của hàm bên trong `who(..)`; hàm đó đóng trên biến `msg`, là tham số từ phạm vi bên ngoài của `greeting(..)`. Khi hàm bên trong đó được trả về, tham chiếu của nó được gán cho biến `hello` trong phạm vi bên ngoài. Sau đó, chúng ta gọi `greeting(..)` lần thứ hai, tạo ra một thể hiện hàm bên trong mới, với một closure mới trên một `msg` mới, và trả về tham chiếu đó để được gán cho `howdy`.

Khi hàm `greeting(..)` chạy xong, thông thường chúng ta sẽ mong đợi tất cả các biến của nó được thu gom rác (xóa khỏi bộ nhớ). Chúng ta mong đợi mỗi `msg` sẽ biến mất, nhưng chúng không. Lý do là closure. Vì các thể hiện hàm bên trong vẫn còn sống (được gán cho `hello` và `howdy`, tương ứng), các closure của chúng vẫn đang bảo tồn các biến `msg`.

Các closure này không phải là ảnh chụp nhanh giá trị của biến `msg`; chúng là một liên kết trực tiếp và bảo tồn chính biến đó. Điều đó có nghĩa là closure thực sự có thể quan sát (hoặc thực hiện!) các cập nhật cho các biến này theo thời gian.

```js
function counter(step = 1) {
    var count = 0;
    return function increaseCount(){
        count = count + step;
        return count;
    };
}

var incBy1 = counter(1);
var incBy3 = counter(3);

incBy1();       // 1
incBy1();       // 2

incBy3();       // 3
incBy3();       // 6
incBy3();       // 9
```

Mỗi thể hiện của hàm bên trong `increaseCount()` được đóng trên cả hai biến `count` và `step` từ phạm vi của hàm bên ngoài `counter(..)` của nó. `step` vẫn giữ nguyên theo thời gian, nhưng `count` được cập nhật trên mỗi lần gọi hàm bên trong đó. Vì closure là trên các biến và không chỉ là ảnh chụp nhanh của các giá trị, các cập nhật này được bảo tồn.

Closure phổ biến nhất khi làm việc với mã không đồng bộ, chẳng hạn như với callbacks. Hãy xem xét:

```js
function getSomeData(url) {
    ajax(url,function onResponse(resp){
        console.log(
            `Response (from ${ url }): ${ resp }`
        );
    });
}

getSomeData("https://some.url/wherever");
// Response (from https://some.url/wherever): ...
```

Hàm bên trong `onResponse(..)` được đóng trên `url`, và do đó bảo tồn và ghi nhớ nó cho đến khi cuộc gọi Ajax trả về và thực thi `onResponse(..)`. Mặc dù `getSomeData(..)` kết thúc ngay lập tức, biến tham số `url` được giữ sống trong closure miễn là cần thiết.

Không nhất thiết phạm vi bên ngoài phải là một hàm—nó thường là vậy, nhưng không phải lúc nào cũng vậy—chỉ cần có ít nhất một biến trong một phạm vi bên ngoài được truy cập từ một hàm bên trong:

```js
for (let [idx,btn] of buttons.entries()) {
    btn.addEventListener("click",function onClick(){
       console.log(`Clicked on button (${ idx })!`);
    });
}
```

Vì vòng lặp này đang sử dụng khai báo `let`, mỗi lần lặp nhận được các biến `idx` và `btn` phạm vi khối (hay còn gọi là, cục bộ) mới; vòng lặp cũng tạo ra một hàm `onClick(..)` bên trong mới mỗi lần. Hàm bên trong đó đóng trên `idx`, bảo tồn nó miễn là trình xử lý nhấp chuột được đặt trên `btn`. Vì vậy, khi mỗi nút được nhấp, trình xử lý của nó có thể in giá trị chỉ mục liên quan của nó, bởi vì trình xử lý ghi nhớ biến `idx` tương ứng của nó.

Hãy nhớ: closure này không phải trên giá trị (như `1` hoặc `3`), mà trên chính biến `idx`.

Closure là một trong những mẫu lập trình phổ biến và quan trọng nhất trong bất kỳ ngôn ngữ nào. Nhưng điều đó đặc biệt đúng với JS; thật khó để tưởng tượng làm bất cứ điều gì hữu ích mà không tận dụng closure theo cách này hay cách khác.

Nếu bạn vẫn cảm thấy không rõ ràng hoặc lung lay về closure, phần lớn Cuốn 2, *Phạm vi & Closures* tập trung vào chủ đề này.

## Từ khóa `this`

Một trong những cơ chế mạnh mẽ nhất của JS cũng là một trong những cơ chế bị hiểu lầm nhiều nhất: từ khóa `this`. Một quan niệm sai lầm phổ biến là `this` của một hàm đề cập đến chính hàm đó. Do cách `this` hoạt động trong các ngôn ngữ khác, một quan niệm sai lầm khác là `this` trỏ đến thể hiện mà một phương thức thuộc về. Cả hai đều không chính xác.

Như đã thảo luận trước đây, khi một hàm được định nghĩa, nó được *gắn* vào phạm vi bao quanh của nó thông qua closure. Phạm vi là tập hợp các quy tắc kiểm soát cách các tham chiếu đến các biến được giải quyết.

Nhưng các hàm cũng có một đặc điểm khác ngoài phạm vi của chúng ảnh hưởng đến những gì chúng có thể truy cập. Đặc điểm này được mô tả tốt nhất là một *ngữ cảnh thực thi* (execution context), và nó được hiển thị cho hàm thông qua từ khóa `this` của nó.

Phạm vi là tĩnh và chứa một tập hợp cố định các biến có sẵn tại thời điểm và vị trí bạn định nghĩa một hàm, nhưng *ngữ cảnh* thực thi của một hàm là động, hoàn toàn phụ thuộc vào **cách nó được gọi** (bất kể nó được định nghĩa ở đâu hoặc thậm chí được gọi từ đâu).

`this` không phải là một đặc điểm cố định của một hàm dựa trên định nghĩa của hàm, mà là một đặc điểm động được xác định mỗi khi hàm được gọi.

Một cách để nghĩ về *ngữ cảnh thực thi* là nó là một đối tượng hữu hình có các thuộc tính được cung cấp cho một hàm trong khi nó thực thi. So sánh điều đó với phạm vi, cũng có thể được coi là một *đối tượng*; ngoại trừ, *đối tượng phạm vi* được ẩn bên trong công cụ JS, nó luôn giống nhau cho hàm đó, và các *thuộc tính* của nó có dạng các biến định danh có sẵn bên trong hàm.

```js
function classroom(teacher) {
    return function study() {
        console.log(
            `${ teacher } says to study ${ this.topic }`
        );
    };
}
var assignment = classroom("Kyle");
```

Hàm bên ngoài `classroom(..)` không tham chiếu đến từ khóa `this`, vì vậy nó giống như bất kỳ hàm nào khác mà chúng ta đã thấy cho đến nay. Nhưng hàm bên trong `study()` có tham chiếu đến `this`, điều này làm cho nó trở thành một hàm nhận biết `this`. Nói cách khác, nó là một hàm phụ thuộc vào *ngữ cảnh thực thi* của nó.

| LƯU Ý: |
| :--- |
| `study()` cũng được đóng trên biến `teacher` từ phạm vi bên ngoài của nó. |

Hàm bên trong `study()` được trả về bởi `classroom("Kyle")` được gán cho một biến gọi là `assignment`. Vậy `assignment()` (hay còn gọi là `study()`) có thể được gọi như thế nào?

```js
assignment();
// Kyle says to study undefined  -- Oops :(
```

Trong đoạn mã này, chúng ta gọi `assignment()` như một hàm bình thường, đơn giản, mà không cung cấp cho nó bất kỳ *ngữ cảnh thực thi* nào.

Vì chương trình này không ở chế độ nghiêm ngặt (xem Chương 1, "Nói một cách nghiêm túc"), các hàm nhận biết ngữ cảnh được gọi **mà không có bất kỳ ngữ cảnh nào được chỉ định** mặc định ngữ cảnh là đối tượng toàn cục (`window` trong trình duyệt). Vì không có biến toàn cục nào có tên `topic` (và do đó không có thuộc tính nào như vậy trên đối tượng toàn cục), `this.topic` giải quyết thành `undefined`.

Bây giờ hãy xem xét:

```js
var homework = {
    topic: "JS",
    assignment: assignment
};

homework.assignment();
// Kyle says to study JS
```

Một bản sao của tham chiếu hàm `assignment` được đặt làm thuộc tính trên đối tượng `homework`, và sau đó nó được gọi là `homework.assignment()`. Điều đó có nghĩa là `this` cho cuộc gọi hàm đó sẽ là đối tượng `homework`. Do đó, `this.topic` giải quyết thành `"JS"`.

Cuối cùng:

```js
var otherHomework = {
    topic: "Math"
};

assignment.call(otherHomework);
// Kyle says to study Math
```

Một cách thứ ba để gọi một hàm là với phương thức `call(..)`, phương thức này nhận một đối tượng (`otherHomework` ở đây) để sử dụng cho việc thiết lập tham chiếu `this` cho cuộc gọi hàm. Tham chiếu thuộc tính `this.topic` giải quyết thành `"Math"`.

Cùng một hàm nhận biết ngữ cảnh được gọi theo ba cách khác nhau, đưa ra các câu trả lời khác nhau mỗi lần cho đối tượng mà `this` sẽ tham chiếu.

Lợi ích của các hàm nhận biết `this`—và ngữ cảnh động của chúng—là khả năng tái sử dụng linh hoạt hơn một hàm duy nhất với dữ liệu từ các đối tượng khác nhau. Một hàm đóng trên một phạm vi không bao giờ có thể tham chiếu đến một phạm vi hoặc tập hợp các biến khác. Nhưng một hàm có nhận thức ngữ cảnh `this` động có thể khá hữu ích cho một số tác vụ nhất định.

## Nguyên mẫu (Prototypes)

Trong khi `this` là một đặc điểm của việc thực thi hàm, thì nguyên mẫu (prototype) là một đặc điểm của một đối tượng, và cụ thể là việc giải quyết truy cập thuộc tính.

Hãy nghĩ về một nguyên mẫu như một liên kết giữa hai đối tượng; liên kết này được ẩn đằng sau hậu trường, mặc dù có nhiều cách để hiển thị và quan sát nó. Liên kết nguyên mẫu này xảy ra khi một đối tượng được tạo ra; nó được liên kết với một đối tượng khác đã tồn tại.

Một chuỗi các đối tượng được liên kết với nhau thông qua các nguyên mẫu được gọi là "chuỗi nguyên mẫu" (prototype chain).

Mục đích của liên kết nguyên mẫu này (tức là, từ một đối tượng B đến một đối tượng A khác) là để các truy cập đối với B cho các thuộc tính/phương thức mà B không có, được *ủy quyền* cho A để xử lý. Việc ủy quyền truy cập thuộc tính/phương thức cho phép hai (hoặc nhiều hơn!) đối tượng hợp tác với nhau để thực hiện một tác vụ.

Hãy xem xét việc định nghĩa một đối tượng như một literal bình thường:

```js
var homework = {
    topic: "JS"
};
```

Đối tượng `homework` chỉ có một thuộc tính duy nhất trên nó: `topic`. Tuy nhiên, liên kết nguyên mẫu mặc định của nó kết nối với đối tượng `Object.prototype`, đối tượng này có các phương thức tích hợp phổ biến trên nó như `toString()` và `valueOf()`, trong số những phương thức khác.

Chúng ta có thể quan sát *sự ủy quyền* liên kết nguyên mẫu này từ `homework` đến `Object.prototype`:

```js
homework.toString();    // [object Object]
```

`homework.toString()` hoạt động ngay cả khi `homework` không có phương thức `toString()` được định nghĩa; sự ủy quyền gọi `Object.prototype.toString()` thay thế.

### Liên kết Đối tượng

Để định nghĩa một liên kết nguyên mẫu đối tượng, bạn có thể tạo đối tượng bằng cách sử dụng tiện ích `Object.create(..)`:

```js
var homework = {
    topic: "JS"
};

var otherHomework = Object.create(homework);

otherHomework.topic;   // "JS"
```

Đối số đầu tiên cho `Object.create(..)` chỉ định một đối tượng để liên kết đối tượng mới được tạo với nó, và sau đó trả về đối tượng mới được tạo (và được liên kết!).

Hình 4 cho thấy cách ba đối tượng (`otherHomework`, `homework`, và `Object.prototype`) được liên kết trong một chuỗi nguyên mẫu:

<figure>
    <img src="images/fig4.png" width="200" alt="Prototype chain with 3 objects" align="center">
    <figcaption><em>Hình 4: Các đối tượng trong một chuỗi nguyên mẫu</em></figcaption>
    <br><br>
</figure>

Việc ủy quyền thông qua chuỗi nguyên mẫu chỉ áp dụng cho các truy cập để tra cứu giá trị trong một thuộc tính. Nếu bạn gán cho một thuộc tính của một đối tượng, điều đó sẽ áp dụng trực tiếp cho đối tượng bất kể đối tượng đó được liên kết nguyên mẫu ở đâu.

| MẸO: |
| :--- |
| `Object.create(null)` tạo ra một đối tượng không được liên kết nguyên mẫu ở bất kỳ đâu, vì vậy nó hoàn toàn chỉ là một đối tượng độc lập; trong một số trường hợp, điều đó có thể thích hợp hơn. |

Hãy xem xét:

```js
homework.topic;
// "JS"

otherHomework.topic;
// "JS"

otherHomework.topic = "Math";
otherHomework.topic;
// "Math"

homework.topic;
// "JS" -- không phải "Math"
```

Việc gán cho `topic` tạo ra một thuộc tính có tên đó trực tiếp trên `otherHomework`; không có ảnh hưởng nào đến thuộc tính `topic` trên `homework`. Câu lệnh tiếp theo sau đó truy cập `otherHomework.topic`, và chúng ta thấy câu trả lời không được ủy quyền từ thuộc tính mới đó: `"Math"`.

Hình 5 cho thấy các đối tượng/thuộc tính sau khi gán tạo ra thuộc tính `otherHomework.topic`:

<figure>
    <img src="images/fig5.png" width="200" alt="3 objects linked, with shadowed property" align="center">
    <figcaption><em>Hình 5: Thuộc tính bị che khuất 'topic'</em></figcaption>
    <br><br>
</figure>

`topic` trên `otherHomework` đang "che khuất" (shadowing) thuộc tính cùng tên trên đối tượng `homework` trong chuỗi.

| LƯU Ý: |
| :--- |
| Một cách khác thẳng thắn là phức tạp hơn nhưng có lẽ vẫn phổ biến hơn để tạo một đối tượng với liên kết nguyên mẫu là sử dụng mẫu "lớp nguyên mẫu" (prototypal class), từ trước khi `class` (xem Chương 2, "Lớp") được thêm vào trong ES6. Chúng ta sẽ đề cập đến chủ đề này chi tiết hơn trong Phụ lục A, "Các 'Lớp' Nguyên mẫu". |

### `this` Xem xét lại

Chúng ta đã đề cập đến từ khóa `this` trước đó, nhưng tầm quan trọng thực sự của nó tỏa sáng khi xem xét cách nó cung cấp năng lượng cho các cuộc gọi hàm được ủy quyền nguyên mẫu. Thật vậy, một trong những lý do chính khiến `this` hỗ trợ ngữ cảnh động dựa trên cách hàm được gọi là để các cuộc gọi phương thức trên các đối tượng ủy quyền thông qua chuỗi nguyên mẫu vẫn duy trì `this` mong đợi.

Hãy xem xét:

```js
var homework = {
    study() {
        console.log(`Please study ${ this.topic }`);
    }
};

var jsHomework = Object.create(homework);
jsHomework.topic = "JS";
jsHomework.study();
// Please study JS

var mathHomework = Object.create(homework);
mathHomework.topic = "Math";
mathHomework.study();
// Please study Math
```

Hai đối tượng `jsHomework` và `mathHomework` mỗi đối tượng liên kết nguyên mẫu với đối tượng `homework` duy nhất, đối tượng này có hàm `study()`. `jsHomework` và `mathHomework` mỗi đối tượng được cung cấp thuộc tính `topic` riêng của chúng (xem Hình 6).

<figure>
    <img src="images/fig6.png" width="495" alt="4 objects prototype linked" align="center">
    <figcaption><em>Hình 6: Hai đối tượng được liên kết với một cha chung</em></figcaption>
    <br><br>
</figure>

`jsHomework.study()` ủy quyền cho `homework.study()`, nhưng `this` (`this.topic`) của nó cho lần thực thi đó giải quyết thành `jsHomework` do cách hàm được gọi, vì vậy `this.topic` là `"JS"`. Tương tự đối với `mathHomework.study()` ủy quyền cho `homework.study()` nhưng vẫn giải quyết `this` thành `mathHomework`, và do đó `this.topic` là `"Math"`.

Đoạn mã trước đó sẽ ít hữu ích hơn nhiều nếu `this` được giải quyết thành `homework`. Tuy nhiên, trong nhiều ngôn ngữ khác, có vẻ như `this` sẽ là `homework` vì phương thức `study()` thực sự được định nghĩa trên `homework`.

Không giống như nhiều ngôn ngữ khác, `this` của JS là động là một thành phần quan trọng cho phép ủy quyền nguyên mẫu, và thực sự là `class`, hoạt động như mong đợi!

## Hỏi "Tại sao?"

Điều cần rút ra từ chương này là có nhiều điều về JS dưới nắp ca-pô hơn là hiển nhiên khi nhìn lướt qua bề mặt.

Khi bạn đang *bắt đầu* học và biết JS kỹ hơn, một trong những kỹ năng quan trọng nhất bạn có thể thực hành và củng cố là sự tò mò, và nghệ thuật hỏi "Tại sao?" khi bạn gặp điều gì đó trong ngôn ngữ.

Mặc dù chương này đã đi khá sâu vào một số chủ đề, nhiều chi tiết vẫn hoàn toàn bị lướt qua. Còn nhiều điều để học ở đây, và con đường đến đó bắt đầu với việc bạn đặt những câu hỏi *đúng* về mã của mình. Đặt những câu hỏi đúng là một kỹ năng quan trọng để trở thành một nhà phát triển giỏi hơn.

Trong chương cuối của cuốn sách này, chúng ta sẽ xem xét ngắn gọn cách JS được phân chia, như được đề cập trong phần còn lại của bộ sách *You Don't Know JS Yet*. Ngoài ra, đừng bỏ qua Phụ lục B của cuốn sách này, trong đó có một số mã thực hành để xem lại một số chủ đề chính được đề cập trong cuốn sách này.
