# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Phụ Lục A: Khám Phá Thêm

Bây giờ chúng ta sẽ khám phá một số sắc thái và góc cạnh xung quanh nhiều chủ đề được đề cập trong văn bản chính của cuốn sách này. Phụ lục này là tài liệu hỗ trợ, tùy chọn.

Một số người thấy việc đi sâu quá mức vào các trường hợp góc sắc thái và các ý kiến khác nhau không tạo ra gì ngoài tiếng ồn và sự xao lãng—được cho là, các nhà phát triển được phục vụ tốt hơn bằng cách bám vào các con đường thường đi. Cách tiếp cận của tôi đã bị chỉ trích là không thực tế và phản tác dụng. Tôi hiểu và đánh giá cao quan điểm đó, ngay cả khi tôi không nhất thiết phải chia sẻ nó.

Tôi tin rằng tốt hơn là được trao quyền bởi kiến thức về cách mọi thứ hoạt động hơn là chỉ lướt qua các chi tiết với các giả định và thiếu tò mò. Cuối cùng, bạn sẽ gặp phải những tình huống mà một cái gì đó nổi lên từ một mảnh bạn chưa khám phá. Nói cách khác, bạn sẽ không dành toàn bộ thời gian của mình để đi trên *con đường hạnh phúc* bằng phẳng. Bạn không muốn chuẩn bị cho những va chạm không thể tránh khỏi của việc đi off-road sao?

Những cuộc thảo luận này cũng sẽ bị ảnh hưởng nặng nề hơn bởi ý kiến của tôi so với văn bản chính, vì vậy hãy ghi nhớ điều đó khi bạn tiêu thụ và xem xét những gì được trình bày. Phụ lục này giống như một bộ sưu tập các bài đăng blog nhỏ giải thích chi tiết về các chủ đề sách khác nhau. Nó dài và sâu trong cỏ dại, vì vậy hãy dành thời gian của bạn và đừng vội vã qua mọi thứ ở đây.

## Phạm Vi Ngụ Ý

Phạm vi đôi khi được tạo ra ở những nơi không rõ ràng. Trong thực tế, các phạm vi ngụ ý này thường không ảnh hưởng đến hành vi chương trình của bạn, nhưng vẫn hữu ích khi biết chúng đang xảy ra. Hãy để mắt đến các phạm vi đáng ngạc nhiên sau:

* Phạm vi tham số
* Phạm vi tên hàm

### Phạm Vi Tham Số

Phép ẩn dụ cuộc trò chuyện trong Chương 2 ngụ ý rằng các tham số hàm về cơ bản giống như các biến được khai báo cục bộ trong phạm vi hàm. Nhưng điều đó không phải lúc nào cũng đúng.

Hãy xem xét:

```js
// phạm vi bên ngoài/toàn cục: RED(1)

function getStudentName(studentID) {
    // phạm vi hàm: BLUE(2)

    // ..
}
```

Ở đây, `studentID` được coi là một tham số "đơn giản", vì vậy nó hoạt động như một thành viên của phạm vi hàm BLUE(2). Nhưng nếu chúng ta thay đổi nó thành một tham số không đơn giản, về mặt kỹ thuật thì không còn là trường hợp đó nữa. Các dạng tham số được coi là không đơn giản bao gồm các tham số có giá trị mặc định, tham số còn lại (sử dụng `...`), và các tham số được giải cấu trúc.

Hãy xem xét:

```js
// phạm vi bên ngoài/toàn cục: RED(1)

function getStudentName(/*BLUE(2)*/ studentID = 0) {
    // phạm vi hàm: GREEN(3)

    // ..
}
```

Ở đây, danh sách tham số về cơ bản trở thành phạm vi riêng của nó, và phạm vi của hàm sau đó được lồng bên trong phạm vi *đó*.

Tại sao? Nó tạo ra sự khác biệt gì? Các dạng tham số không đơn giản giới thiệu các trường hợp góc khác nhau, vì vậy danh sách tham số trở thành phạm vi riêng của nó để đối phó hiệu quả hơn với chúng.

Hãy xem xét:

```js
function getStudentName(studentID = maxID, maxID) {
    // ..
}
```

Giả sử các hoạt động từ trái sang phải, mặc định `= maxID` cho tham số `studentID` yêu cầu `maxID` đã tồn tại (và đã được khởi tạo). Mã này tạo ra lỗi TDZ (Chương 5). Lý do là `maxID` được khai báo trong phạm vi tham số, nhưng nó chưa được khởi tạo vì thứ tự của các tham số. Nếu thứ tự tham số bị đảo ngược, không có lỗi TDZ xảy ra:

```js
function getStudentName(maxID,studentID = maxID) {
    // ..
}
```

Sự phức tạp trở nên *trong cỏ dại* hơn nếu chúng ta giới thiệu một biểu thức hàm vào vị trí tham số mặc định, sau đó có thể tạo closure riêng của nó (Chương 7) trên các tham số trong phạm vi tham số ngụ ý này:

```js
function whatsTheDealHere(id,defaultID = () => id) {
    id = 5;
    console.log( defaultID() );
}

whatsTheDealHere(3);
// 5
```

Đoạn trích đó có lẽ có ý nghĩa, bởi vì hàm mũi tên `defaultID()` đóng trên tham số/biến `id`, mà chúng ta sau đó gán lại thành `5`. Nhưng bây giờ hãy giới thiệu một định nghĩa che khuất của `id` trong phạm vi hàm:

```js
function whatsTheDealHere(id,defaultID = () => id) {
    var id = 5;
    console.log( defaultID() );
}

whatsTheDealHere(3);
// 3
```

Ồ không! `var id = 5` đang che khuất tham số `id`, nhưng closure của hàm `defaultID()` là trên tham số, không phải biến che khuất trong thân hàm. Điều này chứng minh có một bong bóng phạm vi xung quanh danh sách tham số.

Nhưng nó còn điên rồ hơn thế!

```js
function whatsTheDealHere(id,defaultID = () => id) {
    var id;

    console.log(`biến cục bộ 'id': ${ id }`);
    console.log(
        `tham số 'id' (closure): ${ defaultID() }`
    );

    console.log("gán lại 'id' thành 5");
    id = 5;

    console.log(`biến cục bộ 'id': ${ id }`);
    console.log(
        `tham số 'id' (closure): ${ defaultID() }`
    );
}

whatsTheDealHere(3);
// biến cục bộ 'id': 3   <--- Hả!? Kỳ lạ!
// tham số 'id' (closure): 3
// gán lại 'id' thành 5
// biến cục bộ 'id': 5
// tham số 'id' (closure): 3
```

Phần kỳ lạ ở đây là thông báo console đầu tiên. Tại thời điểm đó, biến cục bộ `id` che khuất vừa được khai báo `var id`, mà Chương 5 khẳng định thường được tự động khởi tạo thành `undefined` ở đầu phạm vi của nó. Tại sao nó không in `undefined`?

Trong trường hợp góc cụ thể này (vì lý do tương thích di sản), JS không tự động khởi tạo `id` thành `undefined`, mà thay vào đó thành giá trị của tham số `id` (`3`)!

Mặc dù hai `id` trông có vẻ như tại thời điểm đó chúng là một biến, chúng thực sự vẫn riêng biệt (và trong các phạm vi riêng biệt). Việc gán `id = 5` làm cho sự phân kỳ có thể quan sát được, nơi tham số `id` giữ nguyên `3` và biến cục bộ trở thành `5`.

Lời khuyên của tôi để tránh bị cắn bởi những sắc thái kỳ lạ này:

* Không bao giờ che khuất các tham số bằng các biến cục bộ

* Tránh sử dụng hàm tham số mặc định đóng trên bất kỳ tham số nào

Ít nhất bây giờ bạn đã biết và có thể cẩn thận về thực tế là danh sách tham số là phạm vi riêng của nó nếu bất kỳ tham số nào là không đơn giản.

### Phạm Vi Tên Hàm

Trong phần "Phạm Vi Tên Hàm" ở Chương 3, tôi đã khẳng định rằng tên của một biểu thức hàm được thêm vào phạm vi riêng của hàm. Hãy nhớ lại:

```js
var askQuestion = function ofTheTeacher(){
    // ..
};
```

Đúng là `ofTheTeacher` không được thêm vào phạm vi bao quanh (nơi `askQuestion` được khai báo), nhưng nó cũng không *chỉ* được thêm vào phạm vi của hàm, theo cách bạn có thể đang giả định. Đó là một trường hợp góc kỳ lạ khác của phạm vi ngụ ý.

Định danh tên của một biểu thức hàm nằm trong phạm vi ngụ ý riêng của nó, lồng giữa phạm vi bao quanh bên ngoài và phạm vi hàm bên trong chính.

Nếu `ofTheTeacher` nằm trong phạm vi của hàm, chúng ta sẽ mong đợi một lỗi ở đây:

```js
var askQuestion = function ofTheTeacher(){
    // tại sao đây không phải là lỗi khai báo trùng lặp?
    let ofTheTeacher = "Confused, yet?";
};
```

Dạng khai báo `let` không cho phép khai báo lại (xem Chương 5). Nhưng đây là che khuất hoàn toàn hợp pháp, không phải khai báo lại, bởi vì hai định danh `ofTheTeacher` nằm trong các phạm vi riêng biệt.

Bạn sẽ hiếm khi gặp bất kỳ trường hợp nào mà phạm vi của định danh tên hàm quan trọng. Nhưng một lần nữa, thật tốt khi biết các cơ chế này thực sự hoạt động như thế nào. Để tránh bị cắn, không bao giờ che khuất các định danh tên hàm.

## Hàm Ẩn Danh vs. Hàm Được Đặt Tên

Như đã thảo luận trong Chương 3, các hàm có thể được thể hiện dưới dạng được đặt tên hoặc ẩn danh. Phổ biến hơn nhiều khi sử dụng dạng ẩn danh, nhưng đó có phải là một ý tưởng hay không?

Khi bạn suy ngẫm về việc đặt tên cho các hàm của mình, hãy xem xét:

* Suy luận tên là không đầy đủ
* Tên từ vựng cho phép tự tham chiếu
* Tên là mô tả hữu ích
* Hàm mũi tên không có tên từ vựng
* IIFE cũng cần tên

### Tên Rõ Ràng hay Được Suy Luận?

Mọi hàm trong chương trình của bạn đều có mục đích. Nếu nó không có mục đích, hãy loại bỏ nó, vì bạn chỉ đang lãng phí không gian. Nếu nó *có* mục đích, *có* một cái tên cho mục đích đó.

Cho đến nay nhiều độc giả có thể đồng ý với tôi. Nhưng điều đó có nghĩa là chúng ta nên luôn đặt cái tên đó vào mã không? Đây là nơi tôi sẽ nhướng mày nhiều hơn một chút. Tôi nói, dứt khoát, có!

Trước hết, "anonymous" hiển thị trong dấu vết ngăn xếp (stack traces) chỉ không hữu ích lắm cho việc gỡ lỗi:

```js
btn.addEventListener("click",function(){
    setTimeout(function(){
        ["a",42].map(function(v){
            console.log(v.toUpperCase());
        });
    },100);
});
// Uncaught TypeError: v.toUpperCase is not a function
//     at myProgram.js:4
//     at Array.map (<anonymous>)
//     at myProgram.js:3
```

Hừm. So sánh với những gì được báo cáo nếu tôi đặt tên cho các hàm:

```js
btn.addEventListener("click",function onClick(){
    setTimeout(function waitAMoment(){
        ["a",42].map(function allUpper(v){
            console.log(v.toUpperCase());
        });
    },100);
});
// Uncaught TypeError: v.toUpperCase is not a function
//     at allUpper (myProgram.js:4)
//     at Array.map (<anonymous>)
//     at waitAMoment (myProgram.js:3)
```

Thấy tên `waitAMoment` và `allUpper` xuất hiện và cung cấp cho dấu vết ngăn xếp thông tin/ngữ cảnh hữu ích hơn cho việc gỡ lỗi như thế nào? Chương trình dễ gỡ lỗi hơn nếu chúng ta sử dụng tên hợp lý cho tất cả các hàm của mình.

| LƯU Ý: |
| :--- |
| Thật không may "&lt;anonymous>" vẫn hiển thị đề cập đến thực tế là việc triển khai `Array.map(..)` không có mặt trong chương trình của chúng ta, nhưng được tích hợp vào công cụ JS. Nó không phải từ bất kỳ sự nhầm lẫn nào mà chương trình của chúng ta giới thiệu với các phím tắt khả năng đọc. |

Nhân tiện, hãy chắc chắn rằng chúng ta đang ở cùng một trang về hàm được đặt tên là gì:

```js
function thisIsNamed() {
    // ..
}

ajax("some.url",function thisIsAlsoNamed(){
   // ..
});

var notNamed = function(){
    // ..
};

makeRequest({
    data: 42,
    cb /* cũng không phải là tên */: function(){
        // ..
    }
});

var stillNotNamed = function butThisIs(){
    // ..
};
```

"Nhưng chờ đã!", bạn nói. Một số trong số đó *được* đặt tên, đúng không!?

```js
var notNamed = function(){
    // ..
};

var config = {
    cb: function(){
        // ..
    }
};

notNamed.name;
// notNamed

config.cb.name;
// cb
```

Những cái này được gọi là tên *được suy luận*. Tên được suy luận là tốt, nhưng chúng không thực sự giải quyết mối quan tâm đầy đủ mà tôi đang thảo luận.

### Thiếu Tên?

Vâng, những tên được suy luận này có thể hiển thị trong dấu vết ngăn xếp, điều này chắc chắn tốt hơn là "anonymous" hiển thị. Nhưng...

```js
function ajax(url,cb) {
    console.log(cb.name);
}

ajax("some.url",function(){
    // ..
});
// ""
```

Rất tiếc. Các biểu thức `function` ẩn danh được truyền dưới dạng callback không có khả năng nhận tên được suy luận, vì vậy `cb.name` chỉ giữ chuỗi rỗng `""`. Đại đa số tất cả các biểu thức `function`, đặc biệt là các biểu thức ẩn danh, được sử dụng làm đối số callback; không ai trong số này nhận được tên. Vì vậy, dựa vào suy luận tên là không đầy đủ, tốt nhất là vậy.

Và không chỉ các callback thiếu sót với suy luận:

```js
var config = {};

config.cb = function(){
    // ..
};

config.cb.name;
// ""

var [ noName ] = [ function(){} ];
noName.name
// ""
```

Bất kỳ phép gán nào của một biểu thức `function` không phải là một *phép gán đơn giản* cũng sẽ thất bại trong việc suy luận tên. Vì vậy, nói cách khác, trừ khi bạn cẩn thận và có chủ ý về nó, về cơ bản hầu hết các biểu thức `function` ẩn danh trong chương trình của bạn thực tế sẽ không có tên nào cả.

Suy luận tên chỉ là... không đủ.

Và ngay cả khi một biểu thức `function` *nhận* được tên được suy luận, điều đó vẫn không được tính là một hàm được đặt tên đầy đủ.

### Tôi Là Ai?

Nếu không có định danh tên từ vựng, hàm không có cách nội bộ nào để tham chiếu đến chính nó. Tự tham chiếu rất quan trọng đối với những thứ như đệ quy và xử lý sự kiện:

```js
// hỏng
runOperation(function(num){
    if (num <= 1) return 1;
    return num * oopsNoNameToCall(num - 1);
});

// cũng hỏng
btn.addEventListener("click",function(){
   console.log("chỉ nên phản hồi một lần nhấp!");
   btn.removeEventListener("click",oopsNoNameHere);
});
```

Bỏ tên từ vựng khỏi callback của bạn làm cho việc tự tham chiếu hàm một cách đáng tin cậy trở nên khó khăn hơn. Bạn *có thể* khai báo một biến trong phạm vi bao quanh tham chiếu đến hàm, nhưng biến này được *kiểm soát* bởi phạm vi bao quanh đó—nó có thể được gán lại, v.v.—vì vậy nó không đáng tin cậy như việc hàm có tự tham chiếu nội bộ riêng của nó.

### Tên Là Bộ Mô Tả

Cuối cùng, và tôi nghĩ quan trọng nhất trong tất cả, bỏ tên khỏi một hàm làm cho người đọc khó biết mục đích của hàm là gì, trong nháy mắt. Họ phải đọc thêm mã, bao gồm mã bên trong hàm, và mã xung quanh bên ngoài hàm, để tìm ra nó.

Hãy xem xét:

```js
[ 1, 2, 3, 4, 5 ].filter(function(v){
    return v % 2 == 1;
});
// [ 1, 3, 5 ]

[ 1, 2, 3, 4, 5 ].filter(function keepOnlyOdds(v){
    return v % 2 == 1;
});
// [ 1, 3, 5 ]
```

Chỉ là không có lý lẽ hợp lý nào để đưa ra rằng **bỏ qua** tên `keepOnlyOdds` khỏi callback đầu tiên giao tiếp hiệu quả hơn với người đọc mục đích của callback này. Bạn đã tiết kiệm được 13 ký tự, nhưng mất thông tin khả năng đọc quan trọng. Tên `keepOnlyOdds` nói rất rõ ràng cho người đọc, trong nháy mắt đầu tiên, điều gì đang xảy ra.

Công cụ JS không quan tâm đến tên. Nhưng những người đọc mã của bạn hoàn toàn quan tâm.

Người đọc có thể nhìn vào `v % 2 == 1` và tìm ra nó đang làm gì không? Chắc chắn. Nhưng họ phải suy luận mục đích (và tên) bằng cách thực thi mã trong đầu. Ngay cả một khoảng dừng ngắn để làm như vậy cũng làm chậm quá trình đọc mã. Một cái tên mô tả tốt làm cho quá trình này gần như dễ dàng và tức thì.

Hãy nghĩ theo cách này: tác giả của mã này cần bao nhiêu lần để tìm ra mục đích của một hàm trước khi thêm tên vào mã? Khoảng một lần. Có thể hai hoặc ba lần nếu họ cần điều chỉnh tên. Nhưng người đọc mã này sẽ phải tìm ra tên/mục đích bao nhiêu lần? Mỗi lần dòng này được đọc. Hàng trăm lần? Hàng ngàn? Hơn nữa?

Bất kể độ dài hay độ phức tạp của hàm, khẳng định của tôi là, tác giả nên tìm ra một cái tên mô tả tốt và thêm nó vào mã. Ngay cả các hàm một dòng trong các câu lệnh `map(..)` và `then(..)` cũng nên được đặt tên:

```js
lookupTheRecords(someData)
.then(function extractSalesRecords(resp){
   return resp.allSales;
})
.then(storeRecords);
```

Tên `extractSalesRecords` nói cho người đọc mục đích của trình xử lý `then(..)` này *tốt hơn* là chỉ suy luận mục đích đó từ việc thực thi trong đầu `return resp.allSales`.

Cái cớ duy nhất để không bao gồm tên trên một hàm là lười biếng (không muốn gõ thêm vài ký tự) hoặc không sáng tạo (không thể nghĩ ra một cái tên hay). Nếu bạn không thể tìm ra một cái tên hay, có thể bạn chưa hiểu đầy đủ về hàm và mục đích của nó. Hàm có lẽ được thiết kế kém, hoặc nó làm quá nhiều việc, và nên được làm lại. Một khi bạn có một hàm được thiết kế tốt, mục đích duy nhất, tên thích hợp của nó sẽ trở nên rõ ràng.

Đây là một mẹo tôi sử dụng: trong khi viết hàm lần đầu tiên, nếu tôi không hiểu đầy đủ mục đích của nó và không thể nghĩ ra một cái tên hay để sử dụng, tôi chỉ sử dụng `TODO` làm tên. Bằng cách đó, sau này khi xem lại mã của mình, tôi có khả năng tìm thấy những trình giữ chỗ tên đó, và tôi có xu hướng (và chuẩn bị tốt hơn!) quay lại và tìm ra một cái tên tốt hơn, thay vì chỉ để nó là `TODO`.

Tất cả các hàm đều cần tên. Mỗi một cái. Không ngoại lệ. Bất kỳ tên nào bạn bỏ qua đều làm cho chương trình khó đọc hơn, khó gỡ lỗi hơn, khó mở rộng và bảo trì sau này hơn.

### Hàm Mũi Tên

Hàm mũi tên **luôn luôn** là ẩn danh, ngay cả khi (hiếm khi) chúng được sử dụng theo cách cung cấp cho chúng một tên được suy luận. Tôi vừa dành vài trang để giải thích tại sao các hàm ẩn danh là một ý tưởng tồi, vì vậy bạn có thể đoán tôi nghĩ gì về các hàm mũi tên.

Đừng sử dụng chúng như một sự thay thế chung cho các hàm thông thường. Chúng ngắn gọn hơn, vâng, nhưng sự ngắn gọn đó đi kèm với chi phí bỏ qua các dấu phân cách trực quan chính giúp não bộ của chúng ta nhanh chóng phân tích những gì chúng ta đang đọc. Và, đối với điểm thảo luận này, chúng là ẩn danh, điều này làm cho chúng tồi tệ hơn về khả năng đọc từ góc độ đó.

Hàm mũi tên có một mục đích, nhưng mục đích đó không phải là để tiết kiệm các lần nhấn phím. Hàm mũi tên có hành vi *lexical this*, điều này nằm ngoài giới hạn thảo luận của chúng ta trong cuốn sách này.

Tóm lại: hàm mũi tên hoàn toàn không định nghĩa từ khóa định danh `this`. Nếu bạn sử dụng một `this` bên trong một hàm mũi tên, nó hoạt động chính xác như bất kỳ tham chiếu biến nào khác, đó là chuỗi phạm vi được tham khảo để tìm một phạm vi hàm (không phải hàm mũi tên) nơi nó *được* định nghĩa, và sử dụng cái đó.

Nói cách khác, hàm mũi tên coi `this` giống như bất kỳ biến từ vựng nào khác.

Nếu bạn đã quen với các thủ thuật như `var self = this`, hoặc nếu bạn thích gọi `.bind(this)` trên các biểu thức `function` bên trong, chỉ để buộc chúng kế thừa một `this` từ một hàm bên ngoài giống như nó là một biến từ vựng, thì các hàm mũi tên `=>` hoàn toàn là lựa chọn tốt hơn. Chúng được thiết kế đặc biệt để khắc phục vấn đề đó.

Vì vậy, trong những trường hợp hiếm hoi bạn cần *lexical this*, hãy sử dụng một hàm mũi tên. Đó là công cụ tốt nhất cho công việc đó. Nhưng chỉ cần biết rằng khi làm như vậy, bạn đang chấp nhận những nhược điểm của một hàm ẩn danh. Bạn nên nỗ lực thêm để giảm thiểu *chi phí* khả năng đọc, chẳng hạn như tên biến mô tả hơn và nhận xét mã.

### Các Biến Thể IIFE

Tất cả các hàm nên có tên. Tôi đã nói điều đó vài lần rồi, đúng không!? Điều đó bao gồm cả IIFE.

```js
(function(){
    // đừng làm điều này!
})();

(function doThisInstead(){
    // ..
})();
```

Làm thế nào chúng ta nghĩ ra một cái tên cho một IIFE? Xác định IIFE đó ở đó để làm gì. Tại sao bạn cần một phạm vi ở vị trí đó? Bạn đang ẩn một biến bộ nhớ cache cho các bản ghi sinh viên?

```js
var getStudents = (function StoreStudentRecords(){
    var studentRecords = [];

    return function getStudents() {
        // ..
    }
})();
```

Tôi đặt tên IIFE là `StoreStudentRecords` vì đó là những gì nó đang làm: lưu trữ các bản ghi sinh viên. Mỗi IIFE nên có một cái tên. Không ngoại lệ.

IIFE thường được định nghĩa bằng cách đặt `( .. )` xung quanh biểu thức `function`, như được hiển thị trong các đoạn trích trước đó. Nhưng đó không phải là cách duy nhất để định nghĩa một IIFE. Về mặt kỹ thuật, lý do duy nhất chúng ta sử dụng tập hợp `( .. )` bao quanh đầu tiên đó chỉ là để từ khóa `function` không ở vị trí đủ điều kiện là một khai báo `function` đối với trình phân tích cú pháp JS. Nhưng có những cách cú pháp khác để tránh bị phân tích cú pháp như một khai báo:

```js
!function thisIsAnIIFE(){
    // ..
}();

+function soIsThisOne(){
    // ..
}();

~function andThisOneToo(){
    // ..
}();
```

Các toán tử `!`, `+`, `~`, và một số toán tử một ngôi khác (toán tử có một toán hạng) đều có thể được đặt trước `function` để biến nó thành một biểu thức. Sau đó, cuộc gọi `()` cuối cùng là hợp lệ, làm cho nó trở thành một IIFE.

Tôi thực sự thích sử dụng toán tử một ngôi `void` khi định nghĩa một IIFE độc lập:

```js
void function yepItsAnIIFE() {
    // ..
}();
```

Lợi ích của `void` là, nó giao tiếp rõ ràng ở đầu hàm rằng IIFE này sẽ không trả về bất kỳ giá trị nào.

Tuy nhiên bạn định nghĩa IIFE của mình, hãy thể hiện tình yêu với chúng bằng cách đặt tên cho chúng.

## Hoisting: Hàm và Biến

Chương 5 đã trình bày cả *hoisting hàm* và *hoisting biến*. Vì hoisting thường được trích dẫn là sai lầm trong thiết kế của JS, tôi muốn khám phá ngắn gọn tại sao cả hai hình thức hoisting này *có thể* có lợi và vẫn nên được xem xét.

Hãy xem xét hoisting ở mức độ sâu hơn bằng cách xem xét các giá trị của:

* Mã thực thi trước, khai báo hàm sau
* Vị trí ngữ nghĩa của các khai báo biến

### Hoisting Hàm

Để xem lại, chương trình này hoạt động nhờ *hoisting hàm*:

```js
getStudents();

// ..

function getStudents() {
    // ..
}
```

Khai báo `function` được hoist trong quá trình biên dịch, có nghĩa là `getStudents` là một định danh được khai báo cho toàn bộ phạm vi. Ngoài ra, định danh `getStudents` được tự động khởi tạo với tham chiếu hàm, một lần nữa ở đầu phạm vi.

Tại sao điều này hữu ích? Lý do tôi thích tận dụng *hoisting hàm* là nó đặt mã *thực thi* trong bất kỳ phạm vi nào lên đầu, và bất kỳ khai báo (hàm) nào khác ở dưới. Điều này có nghĩa là dễ dàng tìm thấy mã sẽ chạy trong bất kỳ khu vực nhất định nào, thay vì phải cuộn và cuộn, hy vọng tìm thấy dấu `}` cuối cùng đánh dấu sự kết thúc của một phạm vi/hàm ở đâu đó.

Tôi tận dụng vị trí nghịch đảo này ở tất cả các cấp độ phạm vi:

```js
getStudents();

// *************

function getStudents() {
    var whatever = doSomething();

    // những thứ khác

    return whatever;

    // *************

    function doSomething() {
        // ..
    }
}
```

Khi tôi lần đầu mở một tệp như vậy, dòng đầu tiên là mã thực thi khởi động hành vi của nó. Rất dễ nhận ra! Sau đó, nếu tôi cần đi tìm và kiểm tra `getStudents()`, tôi thích rằng dòng đầu tiên của nó cũng là mã thực thi. Chỉ khi tôi cần xem chi tiết của `doSomething()` tôi mới đi và tìm định nghĩa của nó ở dưới.

Nói cách khác, tôi nghĩ *hoisting hàm* làm cho mã dễ đọc hơn thông qua thứ tự đọc trôi chảy, tiến bộ, từ trên xuống dưới.

### Hoisting Biến

Còn về *hoisting biến* thì sao?

Mặc dù `let` và `const` hoist, bạn không thể sử dụng các biến đó trong TDZ của chúng (xem Chương 5). Vì vậy, cuộc thảo luận sau đây chỉ áp dụng cho các khai báo `var`. Trước khi tôi tiếp tục, tôi thừa nhận: trong hầu hết các trường hợp, tôi hoàn toàn đồng ý rằng *hoisting biến* là một ý tưởng tồi:

```js
pleaseDontDoThis = "bad idea";

// muộn hơn nhiều
var pleaseDontDoThis;
```

Trong khi loại thứ tự đảo ngược đó hữu ích cho *hoisting hàm*, ở đây tôi nghĩ nó thường làm cho mã khó suy luận hơn.

Nhưng có một ngoại lệ mà tôi đã tìm thấy, hơi hiếm, trong mã hóa của riêng tôi. Nó liên quan đến nơi tôi đặt các khai báo `var` của mình bên trong một định nghĩa module CommonJS.

Đây là cách tôi thường cấu trúc các định nghĩa module của mình trong Node:

```js
// phụ thuộc
var aModuleINeed = require("very-helpful");
var anotherModule = require("kinda-helpful");

// API công khai
var publicAPI = Object.assign(module.exports,{
    getStudents,
    addStudents,
    // ..
});

// ********************************
// triển khai riêng tư

var cache = { };
var otherData = [ ];

function getStudents() {
    // ..
}

function addStudents() {
    // ..
}
```

Chú ý cách các biến `cache` và `otherData` nằm trong phần "riêng tư" của bố cục module? Đó là bởi vì tôi không có kế hoạch phơi bày chúng công khai. Vì vậy, tôi tổ chức module để chúng nằm cùng với các chi tiết triển khai ẩn khác của module.

Nhưng tôi đã có một vài trường hợp hiếm hoi mà tôi cần các phép gán của các giá trị đó xảy ra *ở trên*, trước khi tôi khai báo API công khai được xuất khẩu của module. Ví dụ:

```js
// API công khai
var publicAPI = Object.assign(module.exports,{
    getStudents,
    addStudents,
    refreshData: refreshData.bind(null,cache)
});
```

Tôi cần biến `cache` đã được gán một giá trị, bởi vì giá trị đó được sử dụng trong quá trình khởi tạo API công khai (ứng dụng một phần `.bind(..)`).

Tôi có nên chỉ di chuyển `var cache = { .. }` lên trên cùng, phía trên khởi tạo API công khai này không? Chà, có lẽ. Nhưng bây giờ ít rõ ràng hơn rằng `var cache` là một chi tiết triển khai *riêng tư*. Đây là sự thỏa hiệp mà tôi (hơi hiếm khi) sử dụng:

```js
cache = {};   // được sử dụng ở đây, nhưng được khai báo bên dưới

// API công khai
var publicAPI = Object.assign(module.exports,{
    getStudents,
    addStudents,
    refreshData: refreshData.bind(null,cache)
});

// ********************************
// triển khai riêng tư

var cache /* = {}*/;
```

Thấy *hoisting biến* không? Tôi đã khai báo `cache` ở dưới nơi nó thuộc về, một cách hợp lý, nhưng trong trường hợp hiếm hoi này tôi đã sử dụng nó sớm hơn ở trên, trong khu vực mà việc khởi tạo của nó là cần thiết. Tôi thậm chí đã để lại một gợi ý về giá trị được gán cho `cache` trong một nhận xét mã.

Đó thực sự là trường hợp duy nhất tôi từng tìm thấy để tận dụng *hoisting biến* để gán một biến sớm hơn trong một phạm vi so với khai báo của nó. Nhưng tôi nghĩ đó là một ngoại lệ hợp lý để sử dụng một cách thận trọng.

## Trường Hợp Cho `var`

Nói về *hoisting biến*, hãy nói chuyện thực tế một chút về `var`, một nhân vật phản diện yêu thích mà các nhà phát triển thích đổ lỗi cho nhiều tai ương của phát triển JS. Trong Chương 5, chúng ta đã khám phá `let`/`const` và hứa chúng ta sẽ xem xét lại nơi `var` rơi vào toàn bộ hỗn hợp.

Khi tôi trình bày trường hợp, đừng bỏ lỡ:

* `var` chưa bao giờ bị hỏng
* `let` là bạn của bạn
* `const` có tiện ích hạn chế
* Tốt nhất của cả hai thế giới: `var` *và* `let`

### Đừng Vứt Bỏ `var`

`var` vẫn ổn, và hoạt động tốt. Nó đã tồn tại trong 25 năm, và nó sẽ tồn tại và hữu ích và hoạt động trong 25 năm nữa hoặc hơn. Những tuyên bố rằng `var` bị hỏng, không dùng nữa, lỗi thời, nguy hiểm, hoặc được thiết kế kém là sự hùa theo phong trào giả tạo.

Điều đó có nghĩa là `var` là bộ khai báo đúng cho mỗi khai báo trong chương trình của bạn không? Chắc chắn là không. Nhưng nó vẫn có chỗ đứng trong các chương trình của bạn. Từ chối sử dụng nó vì ai đó trong nhóm đã chọn một ý kiến linter hung hăng bóp nghẹt `var` là tự làm hại mình.

OK, bây giờ tôi đã làm bạn thực sự tức giận, hãy để tôi cố gắng giải thích quan điểm của mình.

Để ghi lại, tôi là một người hâm mộ của `let`, cho các khai báo phạm vi khối. Tôi thực sự không thích TDZ và tôi nghĩ đó là một sai lầm. Nhưng bản thân `let` rất tuyệt. Tôi sử dụng nó thường xuyên. Trên thực tế, tôi có lẽ sử dụng nó nhiều hoặc nhiều hơn tôi sử dụng `var`.

### `const`-antly Bối Rối

`const` mặt khác, tôi không sử dụng thường xuyên. Tôi sẽ không đào sâu vào tất cả các lý do tại sao, nhưng nó quy về việc `const` không *mang lại trọng lượng của riêng nó*. Nghĩa là, trong khi có một chút lợi ích của `const` trong một số trường hợp, lợi ích đó bị lu mờ bởi lịch sử lâu dài của những rắc rối xung quanh sự nhầm lẫn `const` trong nhiều ngôn ngữ, rất lâu trước khi nó xuất hiện trong JS.

`const` giả vờ tạo ra các giá trị không thể thay đổi—một quan niệm sai lầm cực kỳ phổ biến trong cộng đồng nhà phát triển trên nhiều ngôn ngữ—trong khi những gì nó thực sự làm là ngăn chặn việc gán lại.

```js
const studentIDs = [ 14, 73, 112 ];

// sau đó

studentIDs.push(6);   // ôi, chờ đã... cái gì!?
```

Sử dụng một `const` với một giá trị có thể thay đổi (như một mảng hoặc đối tượng) là yêu cầu một nhà phát triển tương lai (hoặc người đọc mã của bạn) rơi vào cái bẫy bạn đặt ra, đó là họ hoặc không biết, hoặc đại loại quên, rằng *tính bất biến giá trị* hoàn toàn không giống với *tính bất biến gán*.

Tôi chỉ không nghĩ chúng ta nên đặt những cái bẫy đó. Lần duy nhất tôi sử dụng `const` là khi tôi gán một giá trị đã bất biến (như `42` hoặc `"Hello, friends!"`), và khi nó rõ ràng là một "hằng số" theo nghĩa là một trình giữ chỗ được đặt tên cho một giá trị literal, cho mục đích ngữ nghĩa. Đó là những gì `const` được sử dụng tốt nhất. Tuy nhiên, điều đó khá hiếm trong mã của tôi.

Nếu việc gán lại biến là một vấn đề lớn, thì `const` sẽ hữu ích hơn. Nhưng việc gán lại biến chỉ không phải là vấn đề lớn về mặt gây ra lỗi. Có một danh sách dài những thứ dẫn đến lỗi trong các chương trình, nhưng "vô tình gán lại" nằm rất, rất xa trong danh sách đó.

Kết hợp điều đó với thực tế là `const` (và `let`) được cho là được sử dụng trong các khối, và các khối được cho là ngắn, và bạn có một khu vực thực sự nhỏ trong mã của mình nơi một khai báo `const` thậm chí có thể áp dụng. Một `const` ở dòng 1 của khối mười dòng của bạn chỉ cho bạn biết điều gì đó về chín dòng tiếp theo. Và điều nó nói với bạn đã rõ ràng bằng cách liếc xuống chín dòng đó: biến không bao giờ ở phía bên trái của một `=`; nó không được gán lại.

Đó là tất cả, đó là tất cả những gì `const` thực sự làm. Ngoài ra, nó không hữu ích lắm. So với sự nhầm lẫn đáng kể về tính bất biến giá trị so với gán, `const` mất đi rất nhiều vẻ hào nhoáng của nó.

Một `let` (hoặc `var`!) không bao giờ được gán lại đã là một "hằng số" về mặt hành vi, ngay cả khi nó không có sự đảm bảo của trình biên dịch. Điều đó đủ tốt trong hầu hết các trường hợp.

### `var` *và* `let`

Trong tâm trí tôi, `const` khá hiếm khi hữu ích, vì vậy đây chỉ là cuộc đua song mã giữa `let` và `var`. Nhưng nó cũng không thực sự là một cuộc đua, bởi vì không nhất thiết phải có chỉ một người chiến thắng. Cả hai đều có thể thắng... các cuộc đua khác nhau.

Thực tế là, bạn nên sử dụng cả `var` và `let` trong các chương trình của mình. Chúng không thể thay thế cho nhau: bạn không nên sử dụng `var` ở nơi `let` được yêu cầu, nhưng bạn cũng không nên sử dụng `let` ở nơi `var` là thích hợp nhất.

Vậy chúng ta vẫn nên sử dụng `var` ở đâu? Trong những trường hợp nào nó là một lựa chọn tốt hơn `let`?

Thứ nhất, tôi luôn sử dụng `var` trong phạm vi cấp cao nhất của bất kỳ hàm nào, bất kể đó là ở đầu, giữa hay cuối hàm. Tôi cũng sử dụng `var` trong phạm vi toàn cục, mặc dù tôi cố gắng giảm thiểu việc sử dụng phạm vi toàn cục.

Tại sao sử dụng `var` cho phạm vi hàm? Bởi vì đó chính xác là những gì `var` làm. Theo nghĩa đen, không có công cụ nào tốt hơn cho công việc phạm vi hóa hàm một khai báo hơn một bộ khai báo đã, trong 25 năm, làm chính xác điều đó.

Bạn *có thể* sử dụng `let` trong phạm vi cấp cao nhất này, nhưng nó không phải là công cụ tốt nhất cho công việc đó. Tôi cũng thấy rằng nếu bạn sử dụng `let` ở mọi nơi, thì ít rõ ràng hơn những khai báo nào được thiết kế để được bản địa hóa và những khai báo nào được dự định sử dụng trong toàn bộ hàm.

Ngược lại, tôi hiếm khi sử dụng một `var` bên trong một khối. Đó là những gì `let` dành cho. Sử dụng công cụ tốt nhất cho công việc. Nếu bạn thấy một `let`, nó cho bạn biết rằng bạn đang xử lý một khai báo được bản địa hóa. Nếu bạn thấy `var`, nó cho bạn biết rằng bạn đang xử lý một khai báo toàn hàm. Đơn giản như vậy.

```js
function getStudents(data) {
    var studentRecords = [];

    for (let record of data.records) {
        let id = `student-${ record.id }`;
        studentRecords.push({
            id,
            record.name
        });
    }

    return studentRecords;
}
```

Biến `studentRecords` được dự định sử dụng trên toàn bộ hàm. `var` là bộ khai báo tốt nhất để nói với người đọc điều đó. Ngược lại, `record` và `id` được dự định chỉ sử dụng trong phạm vi hẹp hơn của lần lặp vòng lặp, vì vậy `let` là công cụ tốt nhất cho công việc đó.

Ngoài lập luận ngữ nghĩa *công cụ tốt nhất* này, `var` có một vài đặc điểm khác, trong một số trường hợp hạn chế nhất định, làm cho nó mạnh mẽ hơn.

Một ví dụ là khi một vòng lặp sử dụng độc quyền một biến, nhưng mệnh đề điều kiện của nó không thể nhìn thấy các khai báo phạm vi khối bên trong lần lặp:

```js
function commitAction() {
    do {
        let result = commit();
        var done = result && result.code == 1;
    } while (!done);
}
```

Ở đây, `result` rõ ràng chỉ được sử dụng bên trong khối, vì vậy chúng ta sử dụng `let`. Nhưng `done` hơi khác một chút. Nó chỉ hữu ích cho vòng lặp, nhưng mệnh đề `while` không thể nhìn thấy các khai báo `let` xuất hiện bên trong vòng lặp. Vì vậy, chúng ta thỏa hiệp và sử dụng `var`, để `done` được hoist ra phạm vi bên ngoài nơi nó có thể được nhìn thấy.

Sự thay thế—khai báo `done` bên ngoài vòng lặp—tách nó khỏi nơi nó được sử dụng lần đầu tiên, và hoặc cần chọn một giá trị mặc định để gán, hoặc tệ hơn, để nó không được gán và do đó trông mơ hồ đối với người đọc. Tôi nghĩ `var` bên trong vòng lặp là thích hợp hơn ở đây.

Một đặc điểm hữu ích khác của `var` được nhìn thấy với các khai báo bên trong các khối không mong muốn. Các khối không mong muốn là các khối được tạo ra vì cú pháp yêu cầu một khối, nhưng nơi ý định của nhà phát triển không thực sự là tạo ra một phạm vi được bản địa hóa. Minh họa tốt nhất về phạm vi không mong muốn là câu lệnh `try..catch`:

```js
function getStudents() {
    try {
        // không thực sự là một phạm vi khối
        var records = fromCache("students");
    }
    catch (err) {
        // ôi, quay lại mặc định
        var records = [];
    }
    // ..
}
```

Có những cách khác để cấu trúc mã này, vâng. Nhưng tôi nghĩ đây là cách *tốt nhất*, với các sự đánh đổi khác nhau.

Tôi không muốn khai báo `records` (với `var` hoặc `let`) bên ngoài khối `try`, và sau đó gán cho nó trong một hoặc cả hai khối. Tôi thích các khai báo ban đầu luôn càng gần càng tốt (lý tưởng nhất là cùng dòng) với lần sử dụng đầu tiên của biến. Trong ví dụ đơn giản này, đó chỉ là khoảng cách vài dòng, nhưng trong mã thực tế nó có thể phát triển thành nhiều dòng hơn. Khoảng cách càng lớn, càng khó để tìm ra biến nào từ phạm vi nào bạn đang gán cho. `var` được sử dụng tại phép gán thực tế làm cho nó ít mơ hồ hơn.

Cũng chú ý tôi đã sử dụng `var` trong cả hai khối `try` và `catch`. Đó là bởi vì tôi muốn báo hiệu cho người đọc rằng bất kể con đường nào được thực hiện, `records` luôn được khai báo. Về mặt kỹ thuật, điều đó hoạt động vì `var` được hoist một lần đến phạm vi hàm. Nhưng nó vẫn là một tín hiệu ngữ nghĩa tốt đẹp để nhắc nhở người đọc những gì một trong hai `var` đảm bảo. Nếu `var` chỉ được sử dụng trong một trong các khối, và bạn chỉ đang đọc khối kia, bạn sẽ không dễ dàng khám phá ra `records` đến từ đâu.

Theo ý kiến của tôi, đây là một siêu năng lực nhỏ của `var`. Nó không chỉ có thể thoát khỏi các khối `try..catch` không chủ ý, mà nó còn được phép xuất hiện nhiều lần trong phạm vi của một hàm. Bạn không thể làm điều đó với `let`. Nó không xấu, nó thực sự là một tính năng hữu ích nhỏ. Hãy nghĩ về `var` giống như một chú thích khai báo nhắc nhở bạn, mỗi lần sử dụng, biến đến từ đâu. "À ha, đúng rồi, nó thuộc về toàn bộ hàm."

Siêu năng lực chú thích lặp lại này hữu ích trong các trường hợp khác:

```js
function getStudents() {
    var data = [];

    // làm gì đó với data
    // .. 50 dòng mã nữa ..

    // hoàn toàn là một chú thích để nhắc nhở chúng ta
    var data;

    // sử dụng data lần nữa
    // ..
}
```

`var data` thứ hai không phải là khai báo lại `data`, nó chỉ chú thích cho lợi ích của người đọc rằng `data` là một khai báo toàn hàm. Bằng cách đó, người đọc không cần phải cuộn lên 50+ dòng mã để tìm khai báo ban đầu.

Tôi hoàn toàn ổn với việc sử dụng lại các biến cho nhiều mục đích trong suốt phạm vi hàm. Tôi cũng hoàn toàn ổn với việc có hai lần sử dụng một biến được phân tách bởi khá nhiều dòng mã. Trong cả hai trường hợp, khả năng "khai báo lại" (chú thích) an toàn với `var` giúp đảm bảo tôi có thể biết `data` của mình đến từ đâu, bất kể tôi ở đâu trong hàm.

Một lần nữa, đáng buồn thay, `let` không thể làm điều này.

Có những sắc thái và kịch bản khác khi `var` hóa ra cung cấp một số hỗ trợ, nhưng tôi sẽ không bàn thêm về điểm này nữa. Điều rút ra là `var` có thể hữu ích trong các chương trình của chúng ta cùng với `let` (và thỉnh thoảng `const`). Bạn có sẵn sàng sử dụng sáng tạo các công cụ mà ngôn ngữ JS cung cấp để kể một câu chuyện phong phú hơn cho người đọc của bạn không?

Đừng chỉ vứt bỏ một công cụ hữu ích như `var` vì ai đó làm bạn xấu hổ khi nghĩ rằng nó không còn tuyệt nữa. Đừng tránh `var` vì bạn đã bị nhầm lẫn một lần nhiều năm trước. Tìm hiểu các công cụ này và sử dụng chúng cho những gì chúng giỏi nhất.

## Vấn Đề Với TDZ Là Gì?

TDZ (vùng chết tạm thời) đã được giải thích trong Chương 5. Chúng ta đã minh họa cách nó xảy ra, nhưng chúng ta đã lướt qua bất kỳ lời giải thích nào về *tại sao* cần thiết phải giới thiệu nó ngay từ đầu. Hãy xem xét ngắn gọn các động lực của TDZ.

Một số manh mối trong câu chuyện nguồn gốc TDZ:

* `const` không bao giờ nên thay đổi
* Tất cả là về thời gian
* `let` có nên hoạt động giống `const` hay `var` hơn không?

### Nơi Tất Cả Bắt Đầu

TDZ đến từ `const`, thực ra.

Trong quá trình phát triển ES6 ban đầu, TC39 phải quyết định xem `const` (và `let`) có hoist lên đầu khối của chúng hay không. Họ quyết định những khai báo này sẽ hoist, tương tự như cách `var` làm. Nếu không phải như vậy, tôi nghĩ một số lo ngại là sự nhầm lẫn với che khuất giữa phạm vi, chẳng hạn như:

```js
let greeting = "Hi!";

{
    // nên in gì ở đây?
    console.log(greeting);

    // .. một loạt các dòng mã ..

    // bây giờ che khuất biến `greeting`
    let greeting = "Hello, friends!";

    // ..
}
```

Chúng ta nên làm gì với câu lệnh `console.log(..)` đó? Có ý nghĩa gì đối với các nhà phát triển JS nếu nó in "Hi!" không? Có vẻ như đó có thể là một cạm bẫy, để có che khuất chỉ khởi động cho nửa sau của khối, nhưng không phải nửa đầu. Đó không phải là hành vi rất trực quan, giống JS. Vì vậy `let` và `const` phải hoist lên đầu khối, hiển thị xuyên suốt.

Nhưng nếu `let` và `const` hoist lên đầu khối (như `var` hoist lên đầu hàm), tại sao `let` và `const` không tự động khởi tạo (thành `undefined`) theo cách `var` làm? Đây là mối quan tâm chính:

```js
{
    // nên in gì ở đây?
    console.log(studentName);

    // sau đó

    const studentName = "Frank";

    // ..
}
```

Hãy tưởng tượng rằng `studentName` không chỉ hoist lên đầu khối này, mà còn được tự động khởi tạo thành `undefined`. Đối với nửa đầu của khối, `studentName` có thể được quan sát có giá trị `undefined`, chẳng hạn như với câu lệnh `console.log(..)` của chúng ta. Một khi câu lệnh `const studentName = ..` đạt được, bây giờ `studentName` được gán `"Frank"`. Từ điểm đó trở đi, `studentName` không bao giờ có thể được gán lại.

Nhưng, có lạ hay ngạc nhiên không khi một hằng số có thể quan sát được có hai giá trị khác nhau, đầu tiên là `undefined`, sau đó là `"Frank"`? Điều đó dường như đi ngược lại những gì chúng ta nghĩ một hằng số (`const`ant) có nghĩa; nó chỉ nên bao giờ được quan sát với một giá trị.

Vì vậy... bây giờ chúng ta có một vấn đề. Chúng ta không thể tự động khởi tạo `studentName` thành `undefined` (hoặc bất kỳ giá trị nào khác cho vấn đề đó). Nhưng biến phải tồn tại trong toàn bộ phạm vi. Chúng ta làm gì với khoảng thời gian từ khi nó tồn tại lần đầu tiên (đầu phạm vi) và khi nó được gán giá trị của nó?

Chúng ta gọi khoảng thời gian này là "vùng chết," như trong "vùng chết tạm thời" (TDZ). Để ngăn chặn sự nhầm lẫn, đã xác định rằng bất kỳ loại truy cập nào của một biến trong khi ở trong TDZ của nó là bất hợp pháp và phải dẫn đến lỗi TDZ.

OK, dòng lý luận đó có ý nghĩa, tôi phải thừa nhận.

### Ai Đã `let` TDZ Ra Ngoài?

Nhưng đó chỉ là `const`. Còn `let` thì sao?

Chà, TC39 đã đưa ra quyết định: vì chúng ta cần một TDZ cho `const`, chúng ta cũng có thể có một TDZ cho `let`. *Thực tế, nếu chúng ta làm cho let có một TDZ, thì chúng ta không khuyến khích tất cả những người làm hoisting biến xấu xí đó làm.* Vì vậy, có một quan điểm nhất quán và, có lẽ, một chút kỹ thuật xã hội để thay đổi hành vi của các nhà phát triển.

Phản biện của tôi sẽ là: nếu bạn đang ủng hộ sự nhất quán, hãy nhất quán với `var` thay vì `const`; `let` chắc chắn giống `var` hơn `const`. Điều đó đặc biệt đúng vì họ đã chọn sự nhất quán với `var` cho toàn bộ điều hoisting-lên-đầu-phạm-vi. Hãy để `const` là thỏa thuận độc đáo của riêng nó với một TDZ, và để câu trả lời cho TDZ hoàn toàn là: chỉ cần tránh TDZ bằng cách luôn khai báo các hằng số của bạn ở đầu phạm vi. Tôi nghĩ điều này sẽ hợp lý hơn.

Nhưng than ôi, đó không phải là cách nó hạ cánh. `let` có một TDZ vì `const` cần một TDZ, vì `let` và `const` bắt chước `var` trong việc hoisting của chúng lên đầu phạm vi (khối). Đó là nó. Quá vòng vo? Đọc lại vài lần.

## Các Callback Đồng Bộ Có Còn Là Closures Không?

Chương 7 đã trình bày hai mô hình khác nhau để giải quyết closure:

* Closure là một thể hiện hàm nhớ các biến bên ngoài của nó ngay cả khi hàm đó được truyền xung quanh và **được gọi trong** các phạm vi khác.

* Closure là một thể hiện hàm và môi trường phạm vi của nó được bảo tồn tại chỗ trong khi bất kỳ tham chiếu nào đến nó được truyền xung quanh và **được gọi từ** các phạm vi khác.

Các mô hình này không khác biệt quá nhiều, nhưng chúng tiếp cận từ một quan điểm khác. Và quan điểm khác đó thay đổi những gì chúng ta xác định là một closure.

Đừng bị lạc khi theo dõi dấu vết thỏ này qua các closure và callback:

* Gọi lại cái gì (hoặc ở đâu)?
* Có lẽ "callback đồng bộ" không phải là nhãn tốt nhất
* Các hàm ***IIF*** không di chuyển xung quanh, tại sao chúng cần closure?
* Trì hoãn theo thời gian là chìa khóa cho closure

### Callback Là Gì?

Trước khi chúng ta xem xét lại closure, hãy để tôi dành một chút thời gian ngắn giải quyết từ "callback." Đó là một chuẩn mực thường được chấp nhận rằng nói "callback" đồng nghĩa với cả *callback bất đồng bộ* và *callback đồng bộ*. Tôi không nghĩ tôi đồng ý rằng đây là một ý tưởng hay, vì vậy tôi muốn giải thích tại sao và đề xuất chúng ta chuyển từ đó sang một thuật ngữ khác.

Đầu tiên hãy xem xét một *callback bất đồng bộ*, một tham chiếu hàm sẽ được gọi tại một điểm *muộn hơn* trong tương lai. "Callback" có nghĩa là gì, trong trường hợp này?

Nó có nghĩa là mã hiện tại đã kết thúc hoặc tạm dừng, tự đình chỉ, và khi hàm đang được đề cập được gọi sau đó, việc thực thi đang nhập lại vào chương trình bị đình chỉ, tiếp tục nó. Cụ thể, điểm nhập lại là mã được bọc trong tham chiếu hàm:

```js
setTimeout(function waitForASecond(){
    // đây là nơi JS nên gọi lại vào
    // chương trình khi bộ đếm thời gian đã trôi qua
},1000);

// đây là nơi chương trình hiện tại kết thúc
// hoặc đình chỉ
```

Trong bối cảnh này, "gọi lại" (calling back) có rất nhiều ý nghĩa. Công cụ JS đang tiếp tục chương trình bị đình chỉ của chúng ta bằng cách *gọi lại vào* tại một vị trí cụ thể. OK, vì vậy một callback là bất đồng bộ.

### Callback Đồng Bộ?

Nhưng còn về *callback đồng bộ* thì sao? Hãy xem xét:

```js
function getLabels(studentIDs) {
    return studentIDs.map(
        function formatIDLabel(id){
            return `Student ID: ${
               String(id).padStart(6)
            }`;
        }
    );
}

getLabels([ 14, 73, 112, 6 ]);
// [
//    "Student ID: 000014",
//    "Student ID: 000073",
//    "Student ID: 000112",
//    "Student ID: 000006"
// ]
```

Chúng ta có nên gọi `formatIDLabel(..)` là một callback không? Tiện ích `map(..)` có thực sự *gọi lại* vào chương trình của chúng ta bằng cách gọi hàm chúng ta đã cung cấp không?

Không có gì để *gọi lại vào* theo đúng nghĩa, bởi vì chương trình chưa tạm dừng hoặc thoát. Chúng ta đang truyền một hàm (tham chiếu) từ một phần của chương trình sang một phần khác của chương trình, và sau đó nó được gọi ngay lập tức.

Có các thuật ngữ được thiết lập khác có thể phù hợp với những gì chúng ta đang làm—truyền vào một hàm (tham chiếu) để một phần khác của chương trình có thể gọi nó thay mặt chúng ta. Bạn có thể nghĩ về điều này như *Dependency Injection* (DI) hoặc *Inversion of Control* (IoC).

DI có thể được tóm tắt là truyền (các) phần cần thiết của chức năng cho một phần khác của chương trình để nó có thể gọi chúng để hoàn thành công việc của nó. Đó là một mô tả khá tốt cho cuộc gọi `map(..)` ở trên, phải không? Tiện ích `map(..)` biết lặp qua các giá trị của danh sách, nhưng nó không biết phải *làm gì* với các giá trị đó. Đó là lý do tại sao chúng ta truyền cho nó hàm `formatIDLabel(..)`. Chúng ta truyền vào sự phụ thuộc.

IoC là một khái niệm khá giống, liên quan. Đảo ngược kiểm soát có nghĩa là thay vì khu vực hiện tại của chương trình của bạn kiểm soát những gì đang xảy ra, bạn trao quyền kiểm soát cho một phần khác của chương trình. Chúng ta đã bọc logic để tính toán một chuỗi nhãn trong hàm `formatIDLabel(..)`, sau đó trao quyền kiểm soát gọi cho tiện ích `map(..)`.

Đáng chú ý, Martin Fowler trích dẫn IoC là sự khác biệt giữa một framework và một thư viện: với một thư viện, bạn gọi các hàm của nó; với một framework, nó gọi các hàm của bạn. [^fowlerIOC]

Trong bối cảnh thảo luận của chúng ta, hoặc DI hoặc IoC có thể hoạt động như một nhãn thay thế cho một *callback đồng bộ*.

Nhưng tôi có một đề xuất khác. Hãy gọi (các hàm trước đây được gọi là) *callback đồng bộ*, là *hàm được gọi lẫn nhau* (inter-invoked functions - IIFs). Vâng, chính xác, tôi đang chơi chữ với IIFE. Những loại hàm này được *gọi lẫn nhau*, nghĩa là: một thực thể khác gọi chúng, trái ngược với IIFE, tự gọi chúng ngay lập tức.

Mối quan hệ giữa một *callback bất đồng bộ* và một IIF là gì? Một *callback bất đồng bộ* là một IIF được gọi bất đồng bộ thay vì đồng bộ.

### Closure Đồng Bộ?

Bây giờ chúng ta đã dán nhãn lại *callback đồng bộ* thành IIF, chúng ta có thể quay lại câu hỏi chính của mình: IIF có phải là một ví dụ về closure không? Rõ ràng, IIF sẽ phải tham chiếu (các) biến từ một phạm vi bên ngoài để nó có bất kỳ cơ hội nào trở thành một closure. IIF `formatIDLabel(..)` từ trước đó không tham chiếu bất kỳ biến nào bên ngoài phạm vi riêng của nó, vì vậy nó chắc chắn không phải là một closure.

Còn về một IIF có tham chiếu bên ngoài, đó có phải là closure không?

```js
function printLabels(labels) {
    var list = document.getElementById("labelsList");

    labels.forEach(
        function renderLabel(label){
            var li = document.createElement("li");
            li.innerText = label;
            list.appendChild(li);
        }
    );
}
```

IIF `renderLabel(..)` bên trong tham chiếu `list` từ phạm vi bao quanh, vì vậy nó là một IIF *có thể* có closure. Nhưng đây là nơi định nghĩa/mô hình chúng ta chọn cho closure quan trọng:

* Nếu `renderLabel(..)` là một **hàm được truyền đi nơi khác**, và hàm đó sau đó được gọi, thì có, `renderLabel(..)` đang thực hiện một closure, bởi vì closure là những gì bảo tồn quyền truy cập của nó vào chuỗi phạm vi ban đầu của nó.

* Nhưng nếu, như trong mô hình khái niệm thay thế từ Chương 7, `renderLabel(..)` ở lại vị trí, và chỉ một tham chiếu đến nó được truyền cho `forEach(..)`, có cần thiết phải có closure để bảo tồn chuỗi phạm vi của `renderLabel(..)`, trong khi nó thực thi đồng bộ ngay bên trong phạm vi riêng của nó không?

Không. Đó chỉ là phạm vi từ vựng bình thường.

Để hiểu tại sao, hãy xem xét dạng thay thế này của `printLabels(..)`:

```js
function printLabels(labels) {
    var list = document.getElementById("labelsList");

    for (let label of labels) {
        // chỉ là một cuộc gọi hàm bình thường trong phạm vi
        // riêng của nó, phải không? Đó không thực sự là closure!
        renderLabel(label);
    }

    // **************

    function renderLabel(label) {
        var li = document.createElement("li");
        li.innerText = label;
        list.appendChild(li);
    }
}
```

Hai phiên bản này của `printLabels(..)` về cơ bản là giống nhau.

Cái sau chắc chắn không phải là một ví dụ về closure, ít nhất là không theo bất kỳ nghĩa hữu ích hoặc có thể quan sát nào. Nó chỉ là phạm vi từ vựng. Phiên bản trước, với `forEach(..)` gọi tham chiếu hàm của chúng ta, về cơ bản là cùng một điều. Nó cũng không phải là closure, mà chỉ là một cuộc gọi hàm phạm vi từ vựng cũ kỹ.

### Trì Hoãn Đến Closure

Nhân tiện, Chương 7 đã đề cập ngắn gọn về ứng dụng một phần và currying (những thứ *có* dựa vào closure!). Đây là một kịch bản thú vị nơi currying thủ công có thể được sử dụng:

```js
function printLabels(labels) {
    var list = document.getElementById("labelsList");
    var renderLabel = renderTo(list);

    // chắc chắn là closure lần này!
    labels.forEach( renderLabel );

    // **************

    function renderTo(list) {
        return function createLabel(label){
            var li = document.createElement("li");
            li.innerText = label;
            list.appendChild(li);
        };
    }
}
```

Hàm bên trong `createLabel(..)`, mà chúng ta gán cho `renderLabel`, được đóng trên `list`, vì vậy closure chắc chắn đang được sử dụng.

Closure cho phép chúng ta nhớ `list` cho sau này, trong khi chúng ta trì hoãn việc thực thi logic tạo nhãn thực tế từ cuộc gọi `renderTo(..)` đến các lần gọi `forEach(..)` tiếp theo của IIF `createLabel(..)`. Điều đó có thể chỉ là một khoảnh khắc ngắn ở đây, nhưng bất kỳ khoảng thời gian nào cũng có thể trôi qua, khi closure cầu nối từ cuộc gọi này sang cuộc gọi khác.

## Các Biến Thể Module Cổ Điển

Chương 8 đã giải thích mẫu module cổ điển, có thể trông như thế này:

```js
var StudentList = (function defineModule(Student){
    var elems = [];

    var publicAPI = {
        renderList() {
            // ..
        }
    };

    return publicAPI;

})(Student);
```

Chú ý rằng chúng ta đang truyền `Student` (một thể hiện module khác) vào như một sự phụ thuộc. Nhưng có rất nhiều biến thể hữu ích trên dạng module này mà bạn có thể gặp phải. Một số gợi ý để nhận ra các biến thể này:

* Module có biết về API của chính nó không?
* Ngay cả khi chúng ta sử dụng một trình tải module lạ mắt, nó chỉ là một module cổ điển
* Một số module cần hoạt động phổ quát

### API Của Tôi Ở Đâu?

Đầu tiên, hầu hết các module cổ điển không định nghĩa và sử dụng một `publicAPI` theo cách tôi đã trình bày trong mã này. Thay vào đó, chúng thường trông giống như:

```js
var StudentList = (function defineModule(Student){
    var elems = [];

    return {
        renderList() {
            // ..
        }
    };

})(Student);
```

Sự khác biệt duy nhất ở đây là trực tiếp trả về đối tượng phục vụ như API công khai cho module, trái ngược với việc lưu nó vào một biến `publicAPI` bên trong trước. Đây là cách hầu hết các module cổ điển được định nghĩa.

Nhưng tôi thực sự thích, và luôn sử dụng bản thân mình, dạng `publicAPI` trước đó. Hai lý do:

* `publicAPI` là một bộ mô tả ngữ nghĩa hỗ trợ khả năng đọc bằng cách làm cho mục đích của đối tượng rõ ràng hơn.

* Lưu trữ một biến `publicAPI` bên trong tham chiếu đến cùng một đối tượng API công khai bên ngoài được trả về, có thể hữu ích nếu bạn cần truy cập hoặc sửa đổi API trong suốt vòng đời của module.

    Ví dụ, bạn có thể muốn gọi một trong các hàm được phơi bày công khai, từ bên trong module. Hoặc, bạn có thể muốn thêm hoặc xóa các phương thức tùy thuộc vào các điều kiện nhất định, hoặc cập nhật giá trị của một thuộc tính được phơi bày.

    Dù trường hợp có thể là gì, đối với tôi có vẻ khá ngớ ngẩn nếu chúng ta *không* duy trì một tham chiếu để truy cập API của chính mình. Đúng không?

### Định Nghĩa Module Bất Đồng Bộ (AMD)

Một biến thể khác trên dạng module cổ điển là các module kiểu AMD (phổ biến vài năm trước), chẳng hạn như những module được hỗ trợ bởi tiện ích RequireJS:

```js
define([ "./Student" ],function StudentList(Student){
    var elems = [];

    return {
        renderList() {
            // ..
        }
    };
});
```

Nếu bạn nhìn kỹ vào `StudentList(..)`, nó là một hàm nhà máy module cổ điển. Bên trong bộ máy của `define(..)` (được cung cấp bởi RequireJS), hàm `StudentList(..)` được thực thi, truyền cho nó bất kỳ thể hiện module nào khác được khai báo là phụ thuộc. Giá trị trả về là một đối tượng đại diện cho API công khai cho module.

Điều này dựa trên chính xác các nguyên tắc tương tự (bao gồm cách closure hoạt động!) như chúng ta đã khám phá với các module cổ điển.

### Module Phổ Quát (UMD)

Biến thể cuối cùng chúng ta sẽ xem xét là UMD, ít phải là một định dạng cụ thể, chính xác và nhiều hơn là một tập hợp các định dạng rất giống nhau. Nó được thiết kế để tạo ra khả năng tương tác tốt hơn (không cần bất kỳ chuyển đổi công cụ xây dựng nào) cho các module có thể được tải trong trình duyệt, bởi các trình tải kiểu AMD, hoặc trong Node. Cá nhân tôi vẫn xuất bản nhiều thư viện tiện ích của mình bằng cách sử dụng một dạng UMD.

Đây là cấu trúc điển hình của một UMD:

```js
(function UMD(name,context,definition){
    // được tải bởi một trình tải kiểu AMD?
    if (
        typeof define === "function" &&
        define.amd
    ) {
        define(definition);
    }
    // trong Node?
    else if (
        typeof module !== "undefined" &&
        module.exports
    ) {
        module.exports = definition(name,context);
    }
    // giả sử kịch bản trình duyệt độc lập
    else {
        context[name] = definition(name,context);
    }
})("StudentList",this,function DEF(name,context){

    var elems = [];

    return {
        renderList() {
            // ..
        }
    };

});
```

Mặc dù nó có thể trông hơi bất thường, UMD thực sự chỉ là một IIFE.

Điều khác biệt là phần biểu thức `function` chính (ở trên cùng) của IIFE chứa một loạt các câu lệnh `if..else if` để phát hiện môi trường nào trong ba môi trường được hỗ trợ mà module đang được tải vào.

Cuộc gọi `()` cuối cùng thường gọi một IIFE đang được truyền ba đối số: `"StudentsList"`, `this`, và một biểu thức `function` khác. Nếu bạn khớp các đối số đó với các tham số của chúng, bạn sẽ thấy chúng là: `name`, `context`, và `definition`, tương ứng. `"StudentList"` (`name`) là nhãn tên cho module, chủ yếu trong trường hợp nó được định nghĩa là một biến toàn cục. `this` (`context`) thường là `window` (hay còn gọi là, đối tượng toàn cục; xem Chương 4) để định nghĩa module theo tên của nó.

`definition(..)` được gọi để thực sự truy xuất định nghĩa của module, và bạn sẽ nhận thấy rằng, chắc chắn rồi, đó chỉ là một dạng module cổ điển!

Không có nghi ngờ gì rằng tại thời điểm viết bài này, ESM (ES Modules) đang trở nên phổ biến và lan rộng nhanh chóng. Nhưng với hàng triệu và hàng triệu module được viết trong 20 năm qua, tất cả đều sử dụng một số biến thể trước ESM của các module cổ điển, chúng vẫn rất quan trọng để có thể đọc và hiểu khi bạn bắt gặp chúng.

[^fowlerIOC]: *Inversion of Control*, Martin Fowler, https://martinfowler.com/bliki/InversionOfControl.html, 26 June 2005.
