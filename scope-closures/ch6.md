# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Chương 6: Hạn Chế Phơi Bày Phạm Vi

Cho đến nay, trọng tâm của chúng ta là giải thích cơ chế hoạt động của phạm vi và biến. Với nền tảng đó đã vững chắc, sự chú ý của chúng ta nâng lên một mức độ tư duy cao hơn: các quyết định và mẫu mà chúng ta áp dụng trên toàn bộ chương trình.

Để bắt đầu, chúng ta sẽ xem xét cách thức và lý do tại sao chúng ta nên sử dụng các mức độ phạm vi khác nhau (hàm và khối) để tổ chức các biến của chương trình, cụ thể là để giảm sự phơi bày quá mức của phạm vi.

## Phơi Bày Tối Thiểu

Việc các hàm định nghĩa phạm vi riêng của chúng là điều hợp lý. Nhưng tại sao chúng ta cũng cần các khối để tạo phạm vi?

Kỹ thuật phần mềm nêu rõ một nguyên tắc cơ bản, thường được áp dụng cho bảo mật phần mềm, được gọi là "Nguyên tắc Đặc quyền Tối thiểu" (POLP). [^POLP] Và một biến thể của nguyên tắc này áp dụng cho cuộc thảo luận hiện tại của chúng ta thường được gọi là "Phơi Bày Tối Thiểu" (POLE).

POLP thể hiện một tư thế phòng thủ đối với kiến trúc phần mềm: các thành phần của hệ thống nên được thiết kế để hoạt động với đặc quyền tối thiểu, truy cập tối thiểu, phơi bày tối thiểu. Nếu mỗi phần được kết nối với các khả năng cần thiết tối thiểu, hệ thống tổng thể sẽ mạnh hơn từ quan điểm bảo mật, bởi vì sự xâm phạm hoặc thất bại của một phần có tác động giảm thiểu đến phần còn lại của hệ thống.

Nếu POLP tập trung vào thiết kế thành phần cấp hệ thống, biến thể POLE *Exposure* tập trung vào mức độ thấp hơn; chúng ta sẽ áp dụng nó cho cách các phạm vi tương tác với nhau.

Khi tuân theo POLE, chúng ta muốn giảm thiểu sự phơi bày của cái gì? Đơn giản là: các biến được đăng ký trong mỗi phạm vi.

Hãy nghĩ theo cách này: tại sao bạn không nên đặt tất cả các biến của chương trình ra ngoài phạm vi toàn cục? Điều đó có lẽ ngay lập tức cảm thấy như một ý tưởng tồi, nhưng đáng để xem xét tại sao lại như vậy. Khi các biến được sử dụng bởi một phần của chương trình được phơi bày cho một phần khác của chương trình, thông qua phạm vi, có ba mối nguy hiểm chính thường phát sinh:

* **Xung Đột Tên**: nếu bạn sử dụng một tên biến/hàm phổ biến và hữu ích trong hai phần khác nhau của chương trình, nhưng định danh đến từ một phạm vi được chia sẻ (như phạm vi toàn cục), thì xung đột tên xảy ra, và rất có thể lỗi sẽ xảy ra khi một phần sử dụng biến/hàm theo cách mà phần kia không mong đợi.

    Ví dụ, hãy tưởng tượng nếu tất cả các vòng lặp của bạn sử dụng một biến chỉ mục `i` toàn cục duy nhất, và sau đó xảy ra trường hợp một vòng lặp trong một hàm đang chạy trong khi lặp lại một vòng lặp từ một hàm khác, và bây giờ biến `i` được chia sẻ nhận được một giá trị không mong đợi.

* **Hành Vi Không Mong Đợi**: nếu bạn phơi bày các biến/hàm mà việc sử dụng chúng là *riêng tư* đối với một phần của chương trình, nó cho phép các nhà phát triển khác sử dụng chúng theo cách bạn không có ý định, điều này có thể vi phạm hành vi mong đợi và gây ra lỗi.

    Ví dụ, nếu phần chương trình của bạn giả định một mảng chứa tất cả các số, nhưng mã của người khác truy cập và sửa đổi mảng để bao gồm booleans và chuỗi, mã của bạn sau đó có thể hoạt động sai theo những cách không mong đợi.

    Tệ hơn nữa, việc phơi bày các chi tiết *riêng tư* mời gọi những người có ý đồ xấu cố gắng lách qua các hạn chế bạn đã áp đặt, để làm những việc với phần mềm của bạn mà không nên được phép.

* **Sự Phụ Thuộc Không Mong Muốn**: nếu bạn phơi bày các biến/hàm một cách không cần thiết, nó mời gọi các nhà phát triển khác sử dụng và phụ thuộc vào những phần *riêng tư* đó. Mặc dù điều đó không phá vỡ chương trình của bạn ngày hôm nay, nó tạo ra một mối nguy hiểm tái cấu trúc trong tương lai, bởi vì bây giờ bạn không thể dễ dàng tái cấu trúc biến hoặc hàm đó mà không có khả năng phá vỡ các phần khác của phần mềm mà bạn không kiểm soát.

    Ví dụ, nếu mã của bạn dựa vào một mảng số, và sau đó bạn quyết định tốt hơn là sử dụng một cấu trúc dữ liệu khác thay vì mảng, bây giờ bạn phải chịu trách nhiệm điều chỉnh các phần bị ảnh hưởng khác của phần mềm.

POLE, khi áp dụng cho phạm vi biến/hàm, về cơ bản nói rằng, mặc định phơi bày mức tối thiểu cần thiết, giữ mọi thứ khác càng riêng tư càng tốt. Khai báo các biến trong các phạm vi nhỏ nhất và lồng sâu nhất có thể, thay vì đặt mọi thứ trong phạm vi toàn cục (hoặc thậm chí phạm vi hàm bên ngoài).

Nếu bạn thiết kế phần mềm của mình phù hợp, bạn có cơ hội lớn hơn nhiều để tránh (hoặc ít nhất là giảm thiểu) ba mối nguy hiểm này.

Hãy xem xét:

```js
function diff(x,y) {
    if (x > y) {
        let tmp = x;
        x = y;
        y = tmp;
    }

    return y - x;
}

diff(3,7);      // 4
diff(7,5);      // 2
```

Trong hàm `diff(..)` này, chúng ta muốn đảm bảo rằng `y` lớn hơn hoặc bằng `x`, để khi chúng ta trừ (`y - x`), kết quả là `0` hoặc lớn hơn. Nếu `x` ban đầu lớn hơn (kết quả sẽ là âm!), chúng ta hoán đổi `x` và `y` bằng cách sử dụng biến `tmp`, để giữ kết quả dương.

Trong ví dụ đơn giản này, có vẻ không quan trọng liệu `tmp` có nằm trong khối `if` hay nó thuộc về cấp độ hàm—nó chắc chắn không nên là một biến toàn cục! Tuy nhiên, theo nguyên tắc POLE, `tmp` nên được ẩn trong phạm vi càng nhiều càng tốt. Vì vậy, chúng ta giới hạn phạm vi khối `tmp` (sử dụng `let`) cho khối `if`.

## Ẩn Trong Phạm Vi (Hàm) Rõ Ràng

Bây giờ nên rõ ràng tại sao việc ẩn các khai báo biến và hàm của chúng ta trong các phạm vi thấp nhất (lồng sâu nhất) có thể là quan trọng. Nhưng chúng ta làm điều đó như thế nào?

Chúng ta đã thấy các từ khóa `let` và `const`, là các bộ khai báo phạm vi khối; chúng ta sẽ quay lại với chúng chi tiết hơn ngay sau đây. Nhưng trước tiên, còn việc ẩn các khai báo `var` hoặc `function` trong các phạm vi thì sao? Điều đó có thể dễ dàng được thực hiện bằng cách bao bọc một phạm vi `function` xung quanh một khai báo.

Hãy xem xét một ví dụ nơi phạm vi `function` có thể hữu ích.

Hoạt động toán học "giai thừa" (ký hiệu là "6!") là phép nhân của một số nguyên đã cho với tất cả các số nguyên thấp hơn liên tiếp xuống đến `1`—thực ra, bạn có thể dừng lại ở `2` vì nhân `1` không làm gì cả. Nói cách khác, "6!" giống như "6 * 5!", giống như "6 * 5 * 4!", v.v. Do bản chất của toán học liên quan, một khi giai thừa của bất kỳ số nguyên nào (như "4!") đã được tính toán, chúng ta không cần phải làm lại công việc đó, vì nó sẽ luôn là cùng một câu trả lời.

Vì vậy, nếu bạn tính toán giai thừa cho `6` một cách ngây thơ, sau đó muốn tính toán giai thừa cho `7`, bạn có thể tính toán lại không cần thiết các giai thừa của tất cả các số nguyên từ 2 đến 6. Nếu bạn sẵn sàng đánh đổi bộ nhớ lấy tốc độ, bạn có thể giải quyết việc tính toán lãng phí đó bằng cách lưu trữ giai thừa của mỗi số nguyên khi nó được tính toán:

```js
var cache = {};

function factorial(x) {
    if (x < 2) return 1;
    if (!(x in cache)) {
        cache[x] = x * factorial(x - 1);
    }
    return cache[x];
}

factorial(6);
// 720

cache;
// {
//     "2": 2,
//     "3": 6,
//     "4": 24,
//     "5": 120,
//     "6": 720
// }

factorial(7);
// 5040
```

Chúng ta đang lưu trữ tất cả các giai thừa đã tính toán trong `cache` để qua nhiều lần gọi đến `factorial(..)`, các tính toán trước đó vẫn còn. Nhưng biến `cache` khá rõ ràng là một chi tiết *riêng tư* về cách `factorial(..)` hoạt động, không phải là thứ nên được phơi bày trong một phạm vi bên ngoài—đặc biệt không phải phạm vi toàn cục.

| LƯU Ý: |
| :--- |
| `factorial(..)` ở đây là đệ quy—một cuộc gọi đến chính nó được thực hiện từ bên trong—nhưng đó chỉ là vì sự ngắn gọn của mã; một triển khai không đệ quy sẽ mang lại cùng một phân tích phạm vi đối với `cache`. |

Tuy nhiên, việc sửa vấn đề phơi bày quá mức này không đơn giản như việc ẩn biến `cache` bên trong `factorial(..)`, như có vẻ như vậy. Vì chúng ta cần `cache` tồn tại qua nhiều lần gọi, nó phải được đặt trong một phạm vi bên ngoài hàm đó. Vậy chúng ta có thể làm gì?

Định nghĩa một phạm vi giữa khác (giữa phạm vi bên ngoài/toàn cục và bên trong của `factorial(..)`) để `cache` được đặt:

```js
// phạm vi bên ngoài/toàn cục

function hideTheCache() {
    // "phạm vi giữa", nơi chúng ta ẩn `cache`
    var cache = {};

    return factorial;

    // **********************

    function factorial(x) {
        // phạm vi bên trong
        if (x < 2) return 1;
        if (!(x in cache)) {
            cache[x] = x * factorial(x - 1);
        }
        return cache[x];
    }
}

var factorial = hideTheCache();

factorial(6);
// 720

factorial(7);
// 5040
```

Hàm `hideTheCache()` không phục vụ mục đích nào khác ngoài việc tạo một phạm vi cho `cache` tồn tại qua nhiều lần gọi đến `factorial(..)`. Nhưng để `factorial(..)` có quyền truy cập vào `cache`, chúng ta phải định nghĩa `factorial(..)` bên trong cùng phạm vi đó. Sau đó, chúng ta trả về tham chiếu hàm, như một giá trị từ `hideTheCache()`, và lưu trữ nó trong một biến phạm vi bên ngoài, cũng có tên `factorial`. Bây giờ khi chúng ta gọi `factorial(..)` (nhiều lần!), `cache` bền vững của nó vẫn ẩn nhưng chỉ có thể truy cập được đối với `factorial(..)`!

OK, nhưng... sẽ rất tẻ nhạt để định nghĩa (và đặt tên!) một phạm vi hàm `hideTheCache(..)` mỗi khi nhu cầu ẩn biến/hàm như vậy xảy ra, đặc biệt là vì chúng ta có thể muốn tránh xung đột tên với hàm này bằng cách đặt cho mỗi lần xuất hiện một tên duy nhất. Ugh.

| LƯU Ý: |
| :--- |
| Kỹ thuật được minh họa—lưu trữ đầu ra đã tính toán của một hàm để tối ưu hóa hiệu suất khi các cuộc gọi lặp lại của cùng đầu vào được mong đợi—khá phổ biến trong thế giới Lập trình Hàm (FP), thường được gọi là "memoization"; việc lưu trữ này dựa vào closure (xem Chương 7). Ngoài ra, có những lo ngại về sử dụng bộ nhớ (được giải quyết trong "Một Lời Về Bộ Nhớ" trong Phụ lục B). Các thư viện FP thường sẽ cung cấp một tiện ích được tối ưu hóa và kiểm tra cho việc memoization của các hàm, sẽ thay thế cho `hideTheCache(..)` ở đây. Memoization nằm ngoài *phạm vi* (chơi chữ!) của cuộc thảo luận của chúng ta, nhưng hãy xem cuốn sách *Functional-Light JavaScript* của tôi để biết thêm thông tin. |

Thay vì định nghĩa một hàm mới và được đặt tên duy nhất mỗi khi một trong những tình huống phạm vi-chỉ-cho-mục-đích-ẩn-biến xảy ra, một giải pháp có lẽ tốt hơn là sử dụng một biểu thức hàm:

```js
var factorial = (function hideTheCache() {
    var cache = {};

    function factorial(x) {
        if (x < 2) return 1;
        if (!(x in cache)) {
            cache[x] = x * factorial(x - 1);
        }
        return cache[x];
    }

    return factorial;
})();

factorial(6);
// 720

factorial(7);
// 5040
```

Đợi đã! Điều này vẫn đang sử dụng một hàm để tạo phạm vi để ẩn `cache`, và trong trường hợp này, hàm vẫn được đặt tên là `hideTheCache`, vậy làm thế nào điều đó giải quyết được bất cứ điều gì?

Nhớ lại từ "Phạm Vi Tên Hàm" (trong Chương 3), điều gì xảy ra với định danh tên từ một biểu thức `function`. Vì `hideTheCache(..)` được định nghĩa là một biểu thức `function` thay vì một khai báo `function`, tên của nó nằm trong phạm vi riêng của nó—về cơ bản là cùng phạm vi với `cache`—thay vì trong phạm vi bên ngoài/toàn cục.

Điều đó có nghĩa là chúng ta có thể đặt tên cho mỗi lần xuất hiện của một biểu thức hàm như vậy cùng một tên chính xác, và không bao giờ có bất kỳ xung đột nào. Thích hợp hơn, chúng ta có thể đặt tên cho mỗi lần xuất hiện theo ngữ nghĩa dựa trên bất cứ điều gì chúng ta đang cố gắng ẩn, và không lo lắng rằng bất kỳ tên nào chúng ta chọn sẽ xung đột với bất kỳ phạm vi biểu thức `function` nào khác trong chương trình.

Trên thực tế, chúng ta *có thể* chỉ cần bỏ tên hoàn toàn—do đó định nghĩa một "biểu thức `function` ẩn danh" thay thế. Nhưng Phụ lục A sẽ thảo luận về tầm quan trọng của tên ngay cả đối với các hàm chỉ phạm vi như vậy.

### Gọi Biểu Thức Hàm Ngay Lập Tức

Có một chút quan trọng khác trong chương trình đệ quy giai thừa trước đó dễ bị bỏ lỡ: dòng ở cuối biểu thức `function` chứa `})();`.

Chú ý rằng chúng ta đã bao quanh toàn bộ biểu thức `function` trong một tập hợp `( .. )`, và sau đó ở cuối, chúng ta đã thêm tập hợp dấu ngoặc đơn `()` thứ hai đó; đó thực sự là gọi biểu thức `function` mà chúng ta vừa định nghĩa. Hơn nữa, trong trường hợp này, tập hợp `( .. )` bao quanh đầu tiên xung quanh biểu thức hàm không hoàn toàn cần thiết (thêm về điều đó trong giây lát), nhưng chúng ta đã sử dụng chúng vì mục đích dễ đọc.

Vì vậy, nói cách khác, chúng ta đang định nghĩa một biểu thức `function` sau đó được gọi ngay lập tức. Mẫu phổ biến này có một tên (rất sáng tạo!): Biểu thức Hàm được Gọi Ngay Lập Tức (Immediately Invoked Function Expression - IIFE).

Một IIFE hữu ích khi chúng ta muốn tạo một phạm vi để ẩn các biến/hàm. Vì nó là một biểu thức, nó có thể được sử dụng ở **bất kỳ** nơi nào trong chương trình JS mà một biểu thức được phép. Một IIFE có thể được đặt tên, như với `hideTheCache()`, hoặc (phổ biến hơn nhiều!) không tên/ẩn danh. Và nó có thể độc lập hoặc, như trước đây, là một phần của câu lệnh khác—`hideTheCache()` trả về tham chiếu hàm `factorial()` sau đó được gán `=` cho biến `factorial`.

Để so sánh, đây là một ví dụ về một IIFE độc lập:

```js
// phạm vi bên ngoài

(function(){
    // phạm vi ẩn bên trong
})();

// thêm phạm vi bên ngoài
```

Không giống như trước đó với `hideTheCache()`, nơi các `(..)` bao quanh bên ngoài được ghi chú là một lựa chọn phong cách tùy chọn, đối với một IIFE độc lập, chúng là **bắt buộc**; chúng phân biệt `function` là một biểu thức, không phải một câu lệnh. Tuy nhiên, để nhất quán, hãy luôn bao quanh một `function` IIFE với `( .. )`.

| LƯU Ý: |
| :--- |
| Về mặt kỹ thuật, các `( .. )` bao quanh không phải là cách cú pháp duy nhất để đảm bảo `function` trong một IIFE được trình phân tích cú pháp JS coi là một biểu thức hàm. Chúng ta sẽ xem xét một số tùy chọn khác trong Phụ lục A. |

#### Ranh Giới Hàm

Hãy cẩn thận rằng việc sử dụng một IIFE để định nghĩa một phạm vi có thể có một số hậu quả không mong muốn, tùy thuộc vào mã xung quanh nó. Bởi vì một IIFE là một hàm đầy đủ, ranh giới hàm thay đổi hành vi của một số câu lệnh/cấu trúc nhất định.

Ví dụ, một câu lệnh `return` trong một đoạn mã nào đó sẽ thay đổi ý nghĩa của nó nếu một IIFE được bao bọc xung quanh nó, bởi vì bây giờ `return` sẽ tham chiếu đến hàm của IIFE. Các IIFE hàm không phải mũi tên cũng thay đổi ràng buộc của từ khóa `this`—thêm về điều đó trong cuốn sách *Objects & Classes*. Và các câu lệnh như `break` và `continue` sẽ không hoạt động qua ranh giới hàm IIFE để kiểm soát một vòng lặp hoặc khối bên ngoài.

Vì vậy, nếu mã bạn cần bao bọc một phạm vi xung quanh có `return`, `this`, `break`, hoặc `continue` trong đó, một IIFE có lẽ không phải là cách tiếp cận tốt nhất. Trong trường hợp đó, bạn có thể tìm cách tạo phạm vi bằng một khối thay vì một hàm.

## Phạm Vi với Khối

Đến thời điểm này, bạn nên cảm thấy khá thoải mái với những lợi ích của việc tạo phạm vi để hạn chế phơi bày định danh.

Cho đến nay, chúng ta đã xem xét việc thực hiện điều này thông qua phạm vi `function` (tức là IIFE). Nhưng bây giờ hãy xem xét việc sử dụng các khai báo `let` với các khối lồng nhau. Nói chung, bất kỳ cặp ngoặc nhọn `{ .. }` nào là một câu lệnh sẽ hoạt động như một khối, nhưng **không nhất thiết** là một phạm vi.

Một khối chỉ trở thành một phạm vi nếu cần thiết, để chứa các khai báo phạm vi khối của nó (tức là `let` hoặc `const`). Hãy xem xét:

```js
{
    // chưa nhất thiết là một phạm vi

    // ..

    // bây giờ chúng ta biết khối cần phải là một phạm vi
    let thisIsNowAScope = true;

    for (let i = 0; i < 5; i++) {
        // đây cũng là một phạm vi, được kích hoạt mỗi
        // lần lặp
        if (i % 2 == 0) {
            // đây chỉ là một khối, không phải một phạm vi
            console.log(i);
        }
    }
}
// 0 2 4
```

Không phải tất cả các cặp ngoặc nhọn `{ .. }` đều tạo khối (và do đó đủ điều kiện để trở thành phạm vi):

* Các literal đối tượng sử dụng các cặp ngoặc nhọn `{ .. }` để phân định danh sách khóa-giá trị của chúng, nhưng các giá trị đối tượng như vậy **không phải** là phạm vi.

* `class` sử dụng ngoặc nhọn `{ .. }` xung quanh định nghĩa thân của nó, nhưng đây không phải là một khối hoặc phạm vi.

* Một `function` sử dụng `{ .. } ` xung quanh thân của nó, nhưng về mặt kỹ thuật đây không phải là một khối—nó là một câu lệnh đơn cho thân hàm. Tuy nhiên, nó *là* một phạm vi (hàm).

* Cặp ngoặc nhọn `{ .. }` trên một câu lệnh `switch` (xung quanh tập hợp các mệnh đề `case`) không định nghĩa một khối/phạm vi.

Ngoài các ví dụ không phải khối như vậy, một cặp ngoặc nhọn `{ .. }` có thể định nghĩa một khối được gắn vào một câu lệnh (như `if` hoặc `for`), hoặc đứng một mình—xem cặp ngoặc nhọn `{ .. }` ngoài cùng trong đoạn mã trước. Một khối rõ ràng loại này—nếu nó không có khai báo, nó thực sự không phải là một phạm vi—không phục vụ mục đích hoạt động nào, mặc dù nó vẫn có thể hữu ích như một tín hiệu ngữ nghĩa.

Các khối `{ .. }` độc lập rõ ràng luôn là cú pháp JS hợp lệ, nhưng vì chúng không thể là một phạm vi trước `let`/`const` của ES6, chúng khá hiếm. Tuy nhiên, sau ES6, chúng bắt đầu phổ biến một chút.

Trong hầu hết các ngôn ngữ hỗ trợ phạm vi khối, một phạm vi khối rõ ràng là một mẫu cực kỳ phổ biến để tạo một lát cắt phạm vi hẹp cho một hoặc một vài biến. Vì vậy, theo nguyên tắc POLE, chúng ta cũng nên nắm lấy mẫu này rộng rãi hơn trong JS; sử dụng phạm vi khối (rõ ràng) để thu hẹp sự phơi bày của các định danh đến mức tối thiểu thực tế.

Một phạm vi khối rõ ràng có thể hữu ích ngay cả bên trong một khối khác (cho dù khối bên ngoài có phải là một phạm vi hay không).

Ví dụ:

```js
if (somethingHappened) {
    // đây là một khối, nhưng không phải một phạm vi

    {
        // đây vừa là một khối vừa là một
        // phạm vi rõ ràng
        let msg = somethingHappened.message();
        notifyOthers(msg);
    }

    // ..

    recoverFromSomething();
}
```

Ở đây, cặp ngoặc nhọn `{ .. }` **bên trong** câu lệnh `if` là một phạm vi khối rõ ràng bên trong thậm chí còn nhỏ hơn cho `msg`, vì biến đó không cần thiết cho toàn bộ khối `if`. Hầu hết các nhà phát triển sẽ chỉ phạm vi khối `msg` cho khối `if` và tiếp tục. Và công bằng mà nói, khi chỉ có một vài dòng để xem xét, đó là một quyết định tung đồng xu. Nhưng khi mã phát triển, các vấn đề phơi bày quá mức này trở nên rõ ràng hơn.

Vậy có đủ quan trọng để thêm cặp `{ .. }` bổ sung và mức thụt lề không? Tôi nghĩ bạn nên tuân theo POLE và luôn (trong giới hạn hợp lý!) định nghĩa khối nhỏ nhất cho mỗi biến. Vì vậy, tôi khuyên bạn nên sử dụng phạm vi khối rõ ràng bổ sung như được hiển thị.

Nhớ lại cuộc thảo luận về lỗi TDZ từ "Biến Chưa Được Khởi Tạo (TDZ)" (Chương 5). Đề xuất của tôi ở đó là: để giảm thiểu rủi ro lỗi TDZ với các khai báo `let`/`const`, hãy luôn đặt các khai báo đó ở đầu phạm vi của chúng.

Nếu bạn thấy mình đặt một khai báo `let` ở giữa một phạm vi, trước tiên hãy nghĩ, "Ôi, không! Cảnh báo TDZ!" Nếu khai báo `let` này không cần thiết trong nửa đầu của khối đó, bạn nên sử dụng một phạm vi khối rõ ràng bên trong để thu hẹp thêm sự phơi bày của nó!

Một ví dụ khác với phạm vi khối rõ ràng:

```js
function getNextMonthStart(dateStr) {
    var nextMonth, year;

    {
        let curMonth;
        [ , year, curMonth ] = dateStr.match(
                /(\d{4})-(\d{2})-\d{2}/
            ) || [];
        nextMonth = (Number(curMonth) % 12) + 1;
    }

    if (nextMonth == 1) {
        year++;
    }

    return `${ year }-${
            String(nextMonth).padStart(2,"0")
        }-01`;
}
getNextMonthStart("2019-12-25");   // 2020-01-01
```

Đầu tiên hãy xác định các phạm vi và các định danh của chúng:

1. Phạm vi bên ngoài/toàn cục có một định danh, hàm `getNextMonthStart(..)`.

2. Phạm vi hàm cho `getNextMonthStart(..)` có ba: `dateStr` (tham số), `nextMonth`, và `year`.

3. Cặp ngoặc nhọn `{ .. }` định nghĩa một phạm vi khối bên trong bao gồm một biến: `curMonth`.

Vậy tại sao đặt `curMonth` trong một phạm vi khối rõ ràng thay vì chỉ bên cạnh `nextMonth` và `year` trong phạm vi hàm cấp cao nhất? Bởi vì `curMonth` chỉ cần thiết cho hai câu lệnh đầu tiên đó; ở cấp độ phạm vi hàm, nó bị phơi bày quá mức.

Ví dụ này nhỏ, vì vậy các mối nguy hiểm của việc phơi bày quá mức `curMonth` khá hạn chế. Nhưng lợi ích của nguyên tắc POLE đạt được tốt nhất khi bạn áp dụng tư duy giảm thiểu phơi bày phạm vi theo mặc định, như một thói quen. Nếu bạn tuân theo nguyên tắc một cách nhất quán ngay cả trong các trường hợp nhỏ, nó sẽ phục vụ bạn nhiều hơn khi các chương trình của bạn phát triển.

Bây giờ hãy xem xét một ví dụ thậm chí còn đáng kể hơn:

```js
function sortNamesByLength(names) {
    var buckets = [];

    for (let firstName of names) {
        if (buckets[firstName.length] == null) {
            buckets[firstName.length] = [];
        }
        buckets[firstName.length].push(firstName);
    }

    // một khối để thu hẹp phạm vi
    {
        let sortedNames = [];

        for (let bucket of buckets) {
            if (bucket) {
                // sắp xếp mỗi xô theo thứ tự chữ cái
                bucket.sort();

                // nối các tên đã sắp xếp vào
                // danh sách đang chạy của chúng ta
                sortedNames = [
                    ...sortedNames,
                    ...bucket
                ];
            }
        }

        return sortedNames;
    }
}

sortNamesByLength([
    "Sally",
    "Suzy",
    "Frank",
    "John",
    "Jennifer",
    "Scott"
]);
// [ "John", "Suzy", "Frank", "Sally",
//   "Scott", "Jennifer" ]
```

Có sáu định danh được khai báo qua năm phạm vi khác nhau. Liệu tất cả các biến này có thể tồn tại trong phạm vi bên ngoài/toàn cục duy nhất không? Về mặt kỹ thuật, có, vì tất cả chúng đều được đặt tên duy nhất và do đó không có xung đột tên. Nhưng điều này sẽ là tổ chức mã thực sự kém, và có khả năng dẫn đến cả sự nhầm lẫn và lỗi trong tương lai.

Chúng ta chia chúng ra thành từng phạm vi lồng nhau bên trong khi thích hợp. Mỗi biến được định nghĩa ở phạm vi trong cùng có thể để chương trình hoạt động như mong muốn.

`sortedNames` có thể đã được định nghĩa trong phạm vi hàm cấp cao nhất, nhưng nó chỉ cần thiết cho nửa sau của hàm này. Để tránh phơi bày quá mức biến đó trong phạm vi cấp cao hơn, chúng ta lại tuân theo POLE và phạm vi khối nó trong phạm vi khối rõ ràng bên trong.

### `var` *và* `let`

Tiếp theo, hãy nói về khai báo `var buckets`. Biến đó được sử dụng trên toàn bộ hàm (ngoại trừ câu lệnh `return` cuối cùng). Bất kỳ biến nào cần thiết trên tất cả (hoặc thậm chí hầu hết) của một hàm nên được khai báo để việc sử dụng như vậy là rõ ràng.

| LƯU Ý: |
| :--- |
| Tham số `names` không được sử dụng trên toàn bộ hàm, nhưng không có cách nào giới hạn phạm vi của một tham số, vì vậy nó hoạt động như một khai báo toàn hàm bất kể. |

Vậy tại sao chúng ta sử dụng `var` thay vì `let` để khai báo biến `buckets`? Có cả lý do ngữ nghĩa và kỹ thuật để chọn `var` ở đây.

Về mặt phong cách, `var` luôn luôn, từ những ngày đầu của JS, báo hiệu "biến thuộc về toàn bộ hàm." Như chúng ta đã khẳng định trong "Phạm Vi Từ Vựng" (Chương 1), `var` gắn vào phạm vi hàm bao quanh gần nhất, bất kể nó xuất hiện ở đâu. Điều đó đúng ngay cả khi `var` xuất hiện bên trong một khối:

```js
function diff(x,y) {
    if (x > y) {
        var tmp = x;    // `tmp` là phạm vi hàm
        x = y;
        y = tmp;
    }

    return y - x;
}
```

Mặc dù `var` nằm bên trong một khối, khai báo của nó là phạm vi hàm (đối với `diff(..)`), không phải phạm vi khối.

Mặc dù bạn có thể khai báo `var` bên trong một khối (và vẫn để nó là phạm vi hàm), tôi sẽ khuyên bạn không nên sử dụng cách tiếp cận này ngoại trừ trong một vài trường hợp cụ thể (được thảo luận trong Phụ lục A). Nếu không, `var` nên được dành riêng để sử dụng trong phạm vi cấp cao nhất của một hàm.

Tại sao không chỉ sử dụng `let` ở cùng vị trí đó? Bởi vì `var` khác biệt về mặt trực quan so với `let` và do đó báo hiệu rõ ràng, "biến này là phạm vi hàm." Sử dụng `let` trong phạm vi cấp cao nhất, đặc biệt nếu không nằm trong vài dòng đầu tiên của một hàm, và khi tất cả các khai báo khác trong các khối sử dụng `let`, không thu hút sự chú ý trực quan đến sự khác biệt với khai báo phạm vi hàm.

Nói cách khác, tôi cảm thấy `var` giao tiếp phạm vi hàm tốt hơn `let`, và `let` vừa giao tiếp (vừa đạt được!) phạm vi khối nơi `var` không đủ. Miễn là các chương trình của bạn sẽ cần cả biến phạm vi hàm và phạm vi khối, cách tiếp cận hợp lý và dễ đọc nhất là sử dụng cả `var` *và* `let` cùng nhau, mỗi cái cho mục đích tốt nhất của riêng chúng.

Có những lý do ngữ nghĩa và hoạt động khác để chọn `var` hoặc `let` trong các kịch bản khác nhau. Chúng ta sẽ khám phá trường hợp cho `var` *và* `let` chi tiết hơn trong Phụ lục A.

| CẢNH BÁO: |
| :--- |
| Khuyến nghị của tôi sử dụng cả `var` *và* `let` rõ ràng là gây tranh cãi và mâu thuẫn với đa số. Phổ biến hơn nhiều khi nghe những khẳng định như, "var bị hỏng, let sửa nó" và, "không bao giờ sử dụng var, let là sự thay thế." Những ý kiến đó là hợp lệ, nhưng chúng chỉ là ý kiến, giống như của tôi. `var` không bị hỏng hoặc lỗi thời về mặt thực tế; nó đã hoạt động từ JS đầu tiên và nó sẽ tiếp tục hoạt động miễn là JS còn tồn tại. |

### Ở Đâu Để `let`?

Lời khuyên của tôi dành `var` cho (hầu hết) chỉ một phạm vi hàm cấp cao nhất có nghĩa là hầu hết các khai báo khác nên sử dụng `let`. Nhưng bạn vẫn có thể tự hỏi làm thế nào để quyết định nơi mỗi khai báo trong chương trình của bạn thuộc về?

POLE đã hướng dẫn bạn về những quyết định đó, nhưng hãy chắc chắn rằng chúng ta tuyên bố rõ ràng. Cách để quyết định không dựa trên từ khóa nào bạn muốn sử dụng. Cách để quyết định là hỏi, "Sự phơi bày phạm vi tối thiểu nhất đủ cho biến này là gì?"

Khi điều đó được trả lời, bạn sẽ biết liệu một biến thuộc về phạm vi khối hay phạm vi hàm. Nếu bạn quyết định ban đầu rằng một biến nên là phạm vi khối, và sau đó nhận ra nó cần được nâng lên thành phạm vi hàm, thì điều đó ra lệnh thay đổi không chỉ vị trí của khai báo biến đó, mà còn cả từ khóa bộ khai báo được sử dụng. Quá trình ra quyết định thực sự nên tiến hành như vậy.

Nếu một khai báo thuộc về phạm vi khối, hãy sử dụng `let`. Nếu nó thuộc về phạm vi hàm, hãy sử dụng `var` (một lần nữa, chỉ là ý kiến của tôi).

Nhưng một cách khác để hình dung việc ra quyết định này là xem xét phiên bản trước ES6 của một chương trình. Ví dụ, hãy nhớ lại `diff(..)` từ trước đó:

```js
function diff(x,y) {
    var tmp;

    if (x > y) {
        tmp = x;
        x = y;
        y = tmp;
    }

    return y - x;
}
```

Trong phiên bản này của `diff(..)`, `tmp` được khai báo rõ ràng trong phạm vi hàm. Điều đó có phù hợp với `tmp` không? Tôi sẽ lập luận, không. `tmp` chỉ cần thiết cho vài câu lệnh đó. Nó không cần thiết cho câu lệnh `return`. Do đó, nó nên là phạm vi khối.

Trước ES6, chúng ta không có `let` nên chúng ta không thể *thực sự* phạm vi khối nó. Nhưng chúng ta có thể làm điều tốt nhất tiếp theo trong việc báo hiệu ý định của mình:

```js
function diff(x,y) {
    if (x > y) {
        // `tmp` vẫn là phạm vi hàm, nhưng
        // vị trí ở đây về mặt ngữ nghĩa
        // báo hiệu phạm vi khối
        var tmp = x;
        x = y;
        y = tmp;
    }

    return y - x;
}
```

Đặt khai báo `var` cho `tmp` bên trong câu lệnh `if` báo hiệu cho người đọc mã rằng `tmp` thuộc về khối đó. Mặc dù JS không thực thi phạm vi đó, tín hiệu ngữ nghĩa vẫn có lợi cho người đọc mã của bạn.

Theo quan điểm này, bạn có thể tìm thấy bất kỳ `var` nào bên trong một khối loại này và chuyển nó sang `let` để thực thi tín hiệu ngữ nghĩa đã được gửi đi. Đó là cách sử dụng hợp lý của `let` theo ý kiến của tôi.

Một ví dụ khác dựa trên lịch sử của `var` nhưng bây giờ hầu như luôn luôn nên sử dụng `let` là vòng lặp `for`:

```js
for (var i = 0; i < 5; i++) {
    // làm gì đó
}
```

Bất kể vòng lặp như vậy được định nghĩa ở đâu, `i` về cơ bản luôn chỉ nên được sử dụng bên trong vòng lặp, trong trường hợp đó POLE ra lệnh nó nên được khai báo với `let` thay vì `var`:

```js
for (let i = 0; i < 5; i++) {
    // làm gì đó
}
```

Hầu như trường hợp duy nhất mà việc chuyển một `var` sang một `let` theo cách này sẽ "phá vỡ" mã của bạn là nếu bạn đang dựa vào việc truy cập trình lặp của vòng lặp (`i`) bên ngoài/sau vòng lặp, chẳng hạn như:

```js
for (var i = 0; i < 5; i++) {
    if (checkValue(i)) {
        break;
    }
}

if (i < 5) {
    console.log("Vòng lặp dừng sớm!");
}
```

Mẫu sử dụng này không phải là hiếm gặp, nhưng hầu hết cảm thấy nó có mùi cấu trúc mã kém. Một cách tiếp cận thích hợp hơn là sử dụng một biến phạm vi bên ngoài khác cho mục đích đó:

```js
var lastI;

for (let i = 0; i < 5; i++) {
    lastI = i;
    if (checkValue(i)) {
        break;
    }
}

if (lastI < 5) {
    console.log("Vòng lặp dừng sớm!");
}
```

`lastI` cần thiết trên toàn bộ phạm vi này, vì vậy nó được khai báo với `var`. `i` chỉ cần thiết trong (mỗi) lần lặp vòng lặp, vì vậy nó được khai báo với `let`.

### Có Gì Đáng Chú Ý?

Cho đến nay chúng ta đã khẳng định rằng `var` và các tham số là phạm vi hàm, và `let`/`const` báo hiệu các khai báo phạm vi khối. Có một ngoại lệ nhỏ cần gọi ra: mệnh đề `catch`.

Kể từ khi giới thiệu `try..catch` trở lại trong ES3 (năm 1999), mệnh đề `catch` đã sử dụng một khả năng khai báo phạm vi khối bổ sung (ít được biết đến):

```js
try {
    doesntExist();
}
catch (err) {
    console.log(err);
    // ReferenceError: 'doesntExist' is not defined
    // ^^^^ thông báo được in từ ngoại lệ đã bắt được

    let onlyHere = true;
    var outerVariable = true;
}

console.log(outerVariable);     // true

console.log(err);
// ReferenceError: 'err' is not defined
// ^^^^ đây là một ngoại lệ được ném ra (không bắt được) khác
```

Biến `err` được khai báo bởi mệnh đề `catch` là phạm vi khối cho khối đó. Khối mệnh đề `catch` này có thể giữ các khai báo phạm vi khối khác thông qua `let`. Nhưng một khai báo `var` bên trong khối này vẫn gắn vào phạm vi hàm/toàn cục bên ngoài.

ES2019 (gần đây, tại thời điểm viết bài) đã thay đổi các mệnh đề `catch` để khai báo của chúng là tùy chọn; nếu khai báo bị bỏ qua, khối `catch` không còn (theo mặc định) là một phạm vi; tuy nhiên, nó vẫn là một khối!

Vì vậy, nếu bạn cần phản ứng với điều kiện *rằng một ngoại lệ đã xảy ra* (để bạn có thể phục hồi một cách duyên dáng), nhưng bạn không quan tâm đến giá trị lỗi chính nó, bạn có thể bỏ qua khai báo `catch`:

```js
try {
    doOptionOne();
}
catch {   // bỏ qua khai báo catch
    doOptionTwoInstead();
}
```

Đây là một sự đơn giản hóa cú pháp nhỏ nhưng thú vị cho một trường hợp sử dụng khá phổ biến, và cũng có thể hiệu quả hơn một chút trong việc loại bỏ một phạm vi không cần thiết!

## Khai Báo Hàm trong Khối (FiB)

Chúng ta đã thấy bây giờ rằng các khai báo sử dụng `let` hoặc `const` là phạm vi khối, và các khai báo `var` là phạm vi hàm. Vậy còn các khai báo `function` xuất hiện trực tiếp bên trong các khối thì sao? Như một tính năng, điều này được gọi là "FiB."

Chúng ta thường nghĩ về các khai báo `function` giống như chúng tương đương với một khai báo `var`. Vậy chúng có phải là phạm vi hàm giống như `var` không?

Không và có. Tôi biết... điều đó thật khó hiểu. Hãy đào sâu vào:

```js
if (false) {
    function ask() {
        console.log("Does this run?");
    }
}
ask();
```

Bạn mong đợi chương trình này sẽ làm gì? Ba kết quả hợp lý:

1. Cuộc gọi `ask()` có thể thất bại với một ngoại lệ `ReferenceError`, bởi vì định danh `ask` là phạm vi khối cho phạm vi khối `if` và do đó không có sẵn trong phạm vi bên ngoài/toàn cục.

2. Cuộc gọi `ask()` có thể thất bại với một ngoại lệ `TypeError`, bởi vì định danh `ask` tồn tại, nhưng nó là `undefined` (vì câu lệnh `if` không chạy) và do đó không phải là một hàm có thể gọi được.

3. Cuộc gọi `ask()` có thể chạy chính xác, in ra thông báo "Does it run?".

Đây là phần khó hiểu: tùy thuộc vào môi trường JS nào bạn thử đoạn mã đó, bạn có thể nhận được kết quả khác nhau! Đây là một trong số ít các khu vực điên rồ nơi hành vi di sản hiện có phản bội một kết quả có thể dự đoán được.

Đặc tả JS nói rằng các khai báo `function` bên trong các khối là phạm vi khối, vì vậy câu trả lời nên là (1). Tuy nhiên, hầu hết các công cụ JS dựa trên trình duyệt (bao gồm v8, đến từ Chrome nhưng cũng được sử dụng trong Node) sẽ hoạt động như (2), có nghĩa là định danh được phạm vi bên ngoài khối `if` nhưng giá trị hàm không được tự động khởi tạo, vì vậy nó vẫn là `undefined`.

Tại sao các công cụ JS trình duyệt được phép hoạt động trái với đặc tả? Bởi vì các công cụ này đã có những hành vi nhất định xung quanh FiB trước khi ES6 giới thiệu phạm vi khối, và có lo ngại rằng việc thay đổi để tuân thủ đặc tả có thể phá vỡ một số mã JS trang web hiện có. Như vậy, một ngoại lệ đã được thực hiện trong Phụ lục B của đặc tả JS, cho phép một số sai lệch nhất định cho các công cụ JS trình duyệt (chỉ!).

| LƯU Ý: |
| :--- |
| Bạn thường sẽ không phân loại Node là môi trường JS trình duyệt, vì nó thường chạy trên máy chủ. Nhưng công cụ v8 của Node được chia sẻ với các trình duyệt Chrome (và Edge). Vì v8 trước hết là một công cụ JS trình duyệt, nó chấp nhận ngoại lệ Phụ lục B này, điều này sau đó có nghĩa là các ngoại lệ trình duyệt được mở rộng sang Node. |

Một trong những trường hợp sử dụng phổ biến nhất để đặt một khai báo `function` trong một khối là để định nghĩa có điều kiện một hàm theo cách này hay cách khác (như với một câu lệnh `if..else`) tùy thuộc vào một số trạng thái môi trường. Ví dụ:

```js
if (typeof Array.isArray != "undefined") {
    function isArray(a) {
        return Array.isArray(a);
    }
}
else {
    function isArray(a) {
        return Object.prototype.toString.call(a)
            == "[object Array]";
    }
}
```

Thật hấp dẫn để cấu trúc mã theo cách này vì lý do hiệu suất, vì kiểm tra `typeof Array.isArray` chỉ được thực hiện một lần, trái ngược với việc định nghĩa chỉ một `isArray(..)` và đặt câu lệnh `if` bên trong nó—việc kiểm tra sau đó sẽ chạy không cần thiết trên mỗi cuộc gọi.

| CẢNH BÁO: |
| :--- |
| Ngoài những rủi ro của sai lệch FiB, một vấn đề khác với định nghĩa có điều kiện của các hàm là khó gỡ lỗi một chương trình như vậy hơn. Nếu bạn kết thúc với một lỗi trong hàm `isArray(..)`, trước tiên bạn phải tìm ra triển khai `isArray(..)` *nào* thực sự đang chạy! Đôi khi, lỗi là cái sai đã được áp dụng vì kiểm tra có điều kiện không chính xác! Nếu bạn định nghĩa nhiều phiên bản của một hàm, chương trình đó luôn khó suy luận và bảo trì hơn. |

Ngoài các đoạn mã trước, một số trường hợp góc FiB khác đang ẩn nấp; các hành vi như vậy trong các trình duyệt khác nhau và các môi trường JS không phải trình duyệt (các công cụ JS không dựa trên trình duyệt) có thể sẽ khác nhau. Ví dụ:

```js
if (true) {
    function ask() {
        console.log("Am I called?");
    }
}

if (true) {
    function ask() {
        console.log("Or what about me?");
    }
}

for (let i = 0; i < 5; i++) {
    function ask() {
        console.log("Or is it one of these?");
    }
}

ask();

function ask() {
    console.log("Wait, maybe, it's this one?");
}
```

Nhớ lại rằng function hoisting như được mô tả trong "Khi Nào Tôi Có Thể Sử Dụng Một Biến?" (trong Chương 5) có thể gợi ý rằng `ask()` cuối cùng trong đoạn mã này, với "Wait, maybe..." là thông báo của nó, sẽ hoist lên trên cuộc gọi đến `ask()`. Vì nó là khai báo hàm cuối cùng của tên đó, nó nên "thắng," đúng không? Thật không may, không.

Tôi không có ý định tài liệu hóa tất cả các trường hợp góc kỳ lạ này, cũng không cố gắng giải thích tại sao mỗi trường hợp trong số chúng lại hoạt động theo một cách nhất định. Thông tin đó, theo ý kiến của tôi, là những chuyện vặt vãnh di sản bí ẩn.

Mối quan tâm thực sự của tôi với FiB là, tôi có thể đưa ra lời khuyên gì để đảm bảo mã của bạn hoạt động có thể dự đoán được trong mọi trường hợp?

Theo như tôi quan tâm, câu trả lời thực tế duy nhất để tránh những thay đổi thất thường của FiB là chỉ cần tránh FiB hoàn toàn. Nói cách khác, không bao giờ đặt một khai báo `function` trực tiếp bên trong bất kỳ khối nào. Luôn đặt các khai báo `function` ở bất cứ đâu trong phạm vi cấp cao nhất của một hàm (hoặc trong phạm vi toàn cục).

Vì vậy, đối với ví dụ `if..else` trước đó, đề xuất của tôi là tránh định nghĩa có điều kiện các hàm nếu có thể. Vâng, nó có thể kém hiệu quả hơn một chút, nhưng đây là cách tiếp cận tổng thể tốt hơn:

```js
function isArray(a) {
    if (typeof Array.isArray != "undefined") {
        return Array.isArray(a);
    }
    else {
        return Object.prototype.toString.call(a)
            == "[object Array]";
    }
}
```

Nếu cú đánh hiệu suất đó trở thành một vấn đề đường dẫn quan trọng cho ứng dụng của bạn, tôi khuyên bạn nên xem xét cách tiếp cận này:

```js
var isArray = function isArray(a) {
    return Array.isArray(a);
};

// ghi đè định nghĩa, nếu bạn phải làm vậy
if (typeof Array.isArray == "undefined") {
    isArray = function isArray(a) {
        return Object.prototype.toString.call(a)
            == "[object Array]";
    };
}
```

Điều quan trọng cần lưu ý là ở đây tôi đang đặt một **biểu thức** `function`, không phải một khai báo, bên trong câu lệnh `if`. Điều đó hoàn toàn tốt và hợp lệ, để các biểu thức `function` xuất hiện bên trong các khối. Cuộc thảo luận của chúng ta về FiB là về việc tránh các **khai báo** `function` trong các khối.

Ngay cả khi bạn kiểm tra chương trình của mình và nó hoạt động chính xác, lợi ích nhỏ bạn có thể nhận được từ việc sử dụng phong cách FiB trong mã của mình bị lu mờ bởi những rủi ro tiềm ẩn trong tương lai cho sự nhầm lẫn của các nhà phát triển khác, hoặc sự khác biệt trong cách mã của bạn chạy trong các môi trường JS khác.

FiB không đáng giá, và nên tránh.

## Kết Thúc Khối

Điểm của các quy tắc phạm vi từ vựng trong một ngôn ngữ lập trình là để chúng ta có thể tổ chức các biến của chương trình một cách thích hợp, cho cả mục đích hoạt động cũng như giao tiếp mã ngữ nghĩa.

Và một trong những kỹ thuật tổ chức quan trọng nhất là đảm bảo rằng không có biến nào bị phơi bày quá mức cho các phạm vi không cần thiết (POLE). Hy vọng bây giờ bạn đánh giá cao phạm vi khối sâu sắc hơn nhiều so với trước đây.

Hy vọng đến bây giờ bạn cảm thấy như bạn đang đứng trên nền tảng vững chắc hơn nhiều với sự hiểu biết về phạm vi từ vựng. Từ cơ sở đó, chương tiếp theo nhảy vào chủ đề nặng nề của closure.

[^POLP]: *Principle of Least Privilege*, https://en.wikipedia.org/wiki/Principle_of_least_privilege, 3 March 2020.
