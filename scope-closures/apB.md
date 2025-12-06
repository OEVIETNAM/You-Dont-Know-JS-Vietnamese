---
layout: default
title: Phụ lục B
parent: Phạm Vi & Closures
nav_order: 11
---

# You Don't Know JS Yet: Phạm Vi & Closures - Ấn bản thứ 2
# Phụ Lục B: Thực Hành

Phụ lục này nhằm cung cấp cho bạn một số bài tập thú vị và đầy thử thách để kiểm tra và củng cố sự hiểu biết của bạn về các chủ đề chính từ cuốn sách này. Đó là một ý tưởng tốt để tự mình thử các bài tập—trong một trình soạn thảo mã thực tế!—thay vì bỏ qua thẳng đến các giải pháp ở cuối. Không gian lận!

Những bài tập này không có một câu trả lời đúng cụ thể mà bạn phải đạt được chính xác. Cách tiếp cận của bạn có thể khác một chút (hoặc rất nhiều!) so với các giải pháp được trình bày, và điều đó là ổn.

Không có sự đánh giá nào về cách bạn viết mã của mình. Hy vọng của tôi là bạn rời khỏi cuốn sách này cảm thấy tự tin rằng bạn có thể giải quyết các loại nhiệm vụ mã hóa này được xây dựng trên một nền tảng kiến thức vững chắc. Đó là mục tiêu duy nhất, ở đây. Nếu bạn hài lòng với mã của mình, tôi cũng vậy!

## Thùng Bi (Buckets of Marbles)

Nhớ Hình 2 từ Chương 2 chứ?

<figure>
    <img src="images/fig2.png" width="300" alt="Bong Bóng Phạm Vi Có Màu" align="center">
    <figcaption><em>Hình 2 (Chương 2): Bong Bóng Phạm Vi Có Màu</em></figcaption>
    <br><br>
</figure>

Bài tập này yêu cầu bạn viết một chương trình—bất kỳ chương trình nào!—chứa các hàm lồng nhau và phạm vi khối, thỏa mãn các ràng buộc sau:

* Nếu bạn tô màu tất cả các phạm vi (bao gồm cả phạm vi toàn cục!) các màu khác nhau, bạn cần ít nhất sáu màu. Hãy chắc chắn thêm một nhận xét mã dán nhãn cho mỗi phạm vi với màu của nó.

    BONUS: xác định bất kỳ phạm vi ngụ ý nào mà mã của bạn có thể có.

* Mỗi phạm vi có ít nhất một định danh.

* Chứa ít nhất hai phạm vi hàm và ít nhất hai phạm vi khối.

* Ít nhất một biến từ phạm vi bên ngoài phải bị che khuất bởi một biến phạm vi lồng nhau (xem Chương 3).

* Ít nhất một tham chiếu biến phải phân giải thành một khai báo biến ở ít nhất hai cấp cao hơn trong chuỗi phạm vi.

| MẸO: |
| :--- |
| Bạn *có thể* chỉ cần viết mã rác loại foo/bar/baz cho bài tập này, nhưng tôi khuyên bạn nên cố gắng nghĩ ra một loại mã thực tế không tầm thường nào đó ít nhất làm điều gì đó hợp lý. |

Hãy tự mình thử bài tập, sau đó xem giải pháp được đề xuất ở cuối phụ lục này.

## Closure (PHẦN 1)

Đầu tiên hãy thực hành closure với một số phép toán máy tính phổ biến: xác định xem một giá trị có phải là số nguyên tố (không có ước số nào khác ngoài 1 và chính nó), và tạo danh sách các thừa số nguyên tố (ước số) cho một số đã cho.

Ví dụ:

```js
isPrime(11);        // true
isPrime(12);        // false

factorize(11);      // [ 11 ]
factorize(12);      // [ 3, 2, 2 ] --> 3*2*2=12
```

Đây là một triển khai của `isPrime(..)`, được điều chỉnh từ thư viện Math.js: [^MathJSisPrime]

```js
function isPrime(v) {
    if (v <= 3) {
        return v > 1;
    }
    if (v % 2 == 0 || v % 3 == 0) {
        return false;
    }
    var vSqrt = Math.sqrt(v);
    for (let i = 5; i <= vSqrt; i += 6) {
        if (v % i == 0 || v % (i + 2) == 0) {
            return false;
        }
    }
    return true;
}
```

Và đây là một triển khai hơi cơ bản của `factorize(..)` (không nên nhầm lẫn với `factorial(..)` từ Chương 6):

```js
function factorize(v) {
    if (!isPrime(v)) {
        let i = Math.floor(Math.sqrt(v));
        while (v % i != 0) {
            i--;
        }
        return [
            ...factorize(i),
            ...factorize(v / i)
        ];
    }
    return [v];
}
```

| LƯU Ý: |
| :--- |
| Tôi gọi đây là cơ bản vì nó không được tối ưu hóa cho hiệu suất. Nó là đệ quy nhị phân (không thể tối ưu hóa gọi đuôi), và nó tạo ra rất nhiều bản sao mảng trung gian. Nó cũng không sắp xếp các thừa số được phát hiện theo bất kỳ cách nào. Có rất nhiều, rất nhiều thuật toán khác cho nhiệm vụ này, nhưng tôi muốn sử dụng một cái gì đó ngắn gọn và dễ hiểu cho bài tập của chúng ta. |

Nếu bạn gọi `isPrime(4327)` nhiều lần trong một chương trình, bạn có thể thấy rằng nó sẽ trải qua tất cả hàng tá bước so sánh/tính toán mỗi lần. Nếu bạn xem xét `factorize(..)`, nó đang gọi `isPrime(..)` nhiều lần khi nó tính toán danh sách các thừa số. Và có khả năng cao hầu hết các cuộc gọi đó là lặp lại. Đó là rất nhiều công việc lãng phí!

Phần đầu tiên của bài tập này là sử dụng closure để triển khai bộ nhớ cache để nhớ kết quả của `isPrime(..)`, để tính nguyên tố (`true` hoặc `false`) của một số nhất định chỉ bao giờ được tính toán một lần. Gợi ý: chúng ta đã trình bày loại bộ nhớ cache này trong Chương 6 với `factorial(..)`.

Nếu bạn nhìn vào `factorize(..)`, nó được triển khai với đệ quy, nghĩa là nó gọi chính nó lặp đi lặp lại. Điều đó một lần nữa có nghĩa là chúng ta có thể thấy rất nhiều cuộc gọi lãng phí để tính toán các thừa số nguyên tố cho cùng một số. Vì vậy, phần thứ hai của bài tập là sử dụng cùng một kỹ thuật bộ nhớ cache closure cho `factorize(..)`.

Sử dụng các closure riêng biệt để lưu trữ bộ nhớ cache của `isPrime(..)` và `factorize(..)`, thay vì đặt chúng bên trong một phạm vi duy nhất.

Hãy tự mình thử bài tập, sau đó xem giải pháp được đề xuất ở cuối phụ lục này.

### Một Lời Về Bộ Nhớ

Tôi muốn chia sẻ một ghi chú nhanh nhỏ về kỹ thuật bộ nhớ cache closure này và những tác động của nó đối với hiệu suất ứng dụng của bạn.

Chúng ta có thể thấy rằng trong việc lưu các cuộc gọi lặp lại, chúng ta cải thiện tốc độ tính toán (trong một số trường hợp, một lượng đáng kể). Nhưng việc sử dụng closure này đang thực hiện một sự đánh đổi rõ ràng mà bạn nên rất ý thức.

Sự đánh đổi là bộ nhớ. Chúng ta về cơ bản đang phát triển bộ nhớ cache của mình (trong bộ nhớ) không giới hạn. Nếu các hàm đang được đề cập được gọi hàng triệu lần với hầu hết các đầu vào duy nhất, chúng ta sẽ ngốn rất nhiều bộ nhớ. Điều này chắc chắn có thể đáng giá chi phí, nhưng chỉ khi chúng ta nghĩ rằng có khả năng chúng ta thấy sự lặp lại của các đầu vào phổ biến để chúng ta đang tận dụng bộ nhớ cache.

Nếu hầu hết mọi cuộc gọi sẽ có một đầu vào duy nhất, và bộ nhớ cache về cơ bản không bao giờ được *sử dụng* cho bất kỳ lợi ích nào, đây là một kỹ thuật không phù hợp để sử dụng.

Cũng có thể là một ý tưởng tốt để có một cách tiếp cận bộ nhớ cache tinh vi hơn, chẳng hạn như bộ nhớ cache LRU (ít được sử dụng gần đây nhất), giới hạn kích thước của nó; khi nó chạy đến giới hạn, một LRU trục xuất các giá trị... chà, ít được sử dụng gần đây nhất!

Nhược điểm ở đây là LRU khá không tầm thường theo đúng nghĩa của nó. Bạn sẽ muốn sử dụng một triển khai LRU được tối ưu hóa cao, và nhận thức sâu sắc về tất cả các sự đánh đổi đang diễn ra.

## Closure (PHẦN 2)

Trong bài tập này, chúng ta sẽ lại thực hành closure bằng cách định nghĩa một tiện ích `toggle(..)` cung cấp cho chúng ta một bộ chuyển đổi giá trị.

Bạn sẽ truyền một hoặc nhiều giá trị (dưới dạng đối số) vào `toggle(..)`, và nhận lại một hàm. Hàm được trả về đó sẽ xen kẽ/xoay vòng giữa tất cả các giá trị được truyền vào theo thứ tự, mỗi lần một giá trị, khi nó được gọi lặp đi lặp lại.

```js
function toggle(/* .. */) {
    // ..
}

var hello = toggle("hello");
var onOff = toggle("on","off");
var speed = toggle("slow","medium","fast");

hello();      // "hello"
hello();      // "hello"

onOff();      // "on"
onOff();      // "off"
onOff();      // "on"

speed();      // "slow"
speed();      // "medium"
speed();      // "fast"
speed();      // "slow"
```

Trường hợp góc của việc không truyền giá trị nào vào `toggle(..)` không quan trọng lắm; một thể hiện bộ chuyển đổi như vậy có thể chỉ luôn trả về `undefined`.

Hãy tự mình thử bài tập, sau đó xem giải pháp được đề xuất ở cuối phụ lục này.

## Closure (PHẦN 3)

Trong bài tập thứ ba và cuối cùng này về closure, chúng ta sẽ triển khai một máy tính cơ bản. Hàm `calculator()` sẽ tạo ra một thể hiện của một máy tính duy trì trạng thái riêng của nó, dưới dạng một hàm (`calc(..)`, bên dưới):

```js
function calculator() {
    // ..
}

var calc = calculator();
```

Mỗi lần `calc(..)` được gọi, bạn sẽ truyền vào một ký tự duy nhất đại diện cho một lần nhấn phím của nút máy tính. Để giữ cho mọi thứ đơn giản hơn, chúng ta sẽ hạn chế máy tính của mình chỉ hỗ trợ nhập các chữ số (0-9), các phép toán số học (+, -, \*, /), và "=" để tính toán phép toán. Các phép toán được xử lý nghiêm ngặt theo thứ tự đã nhập; không có nhóm "( )" hoặc ưu tiên toán tử.

Chúng ta không hỗ trợ nhập số thập phân, nhưng phép chia có thể dẫn đến chúng. Chúng ta không hỗ trợ nhập số âm, nhưng phép toán "-" có thể dẫn đến chúng. Vì vậy, bạn sẽ có thể tạo ra bất kỳ số âm hoặc số thập phân nào bằng cách nhập một phép toán để tính toán nó trước. Sau đó, bạn có thể tiếp tục tính toán với giá trị đó.

Việc trả về của các cuộc gọi `calc(..)` nên bắt chước những gì sẽ được hiển thị trên một máy tính thực, như phản ánh những gì vừa được nhấn, hoặc tính tổng khi nhấn "=".

Ví dụ:

```js
calc("4");     // 4
calc("+");     // +
calc("7");     // 7
calc("3");     // 3
calc("-");     // -
calc("2");     // 2
calc("=");     // 75
calc("*");     // *
calc("4");     // 4
calc("=");     // 300
calc("5");     // 5
calc("-");     // -
calc("5");     // 5
calc("=");     // 0
```

Vì cách sử dụng này hơi vụng về, đây là một trình trợ giúp `useCalc(..)`, chạy máy tính với các ký tự mỗi lần một ký tự từ một chuỗi, và tính toán hiển thị mỗi lần:

```js
function useCalc(calc,keys) {
    return [...keys].reduce(
        function showDisplay(display,key){
            var ret = String( calc(key) );
            return (
                display +
                (
                  (ret != "" && key == "=") ?
                      "=" :
                      ""
                ) +
                ret
            );
        },
        ""
    );
}

useCalc(calc,"4+3=");           // 4+3=7
useCalc(calc,"+9=");            // +9=16
useCalc(calc,"*8=");            // *5=128
useCalc(calc,"7*2*3=");         // 7*2*3=42
useCalc(calc,"1/0=");           // 1/0=ERR
useCalc(calc,"+3=");            // +3=ERR
useCalc(calc,"51=");            // 51
```

Cách sử dụng hợp lý nhất của trình trợ giúp `useCalc(..)` này là luôn có "=" là ký tự cuối cùng được nhập.

Một số định dạng của tổng số được hiển thị bởi máy tính yêu cầu xử lý đặc biệt. Tôi đang cung cấp hàm `formatTotal(..)` này, mà máy tính của bạn nên sử dụng bất cứ khi nào nó sẽ trả về tổng số được tính toán hiện tại (sau khi `"="` được nhập):

```js
function formatTotal(display) {
    if (Number.isFinite(display)) {
        // giới hạn hiển thị tối đa 11 ký tự
        let maxDigits = 11;
        // dành không gian cho ký hiệu "e+"?
        if (Math.abs(display) > 99999999999) {
            maxDigits -= 6;
        }
        // dành không gian cho "-"?
        if (display < 0) {
            maxDigits--;
        }

        // số nguyên?
        if (Number.isInteger(display)) {
            display = display
                .toPrecision(maxDigits)
                .replace(/\.0+$/,"");
        }
        // số thập phân
        else {
            // dành không gian cho "."
            maxDigits--;
            // dành không gian cho số "0" dẫn đầu?
            if (
                Math.abs(display) >= 0 &&
                Math.abs(display) < 1
            ) {
                maxDigits--;
            }
            display = display
                .toPrecision(maxDigits)
                .replace(/0+$/,"");
        }
    }
    else {
        display = "ERR";
    }
    return display;
}
```

Đừng lo lắng quá nhiều về cách `formatTotal(..)` hoạt động. Hầu hết logic của nó là một loạt các xử lý để giới hạn hiển thị máy tính tối đa 11 ký tự, ngay cả khi số âm, số thập phân lặp lại, hoặc thậm chí ký hiệu mũ "e+" là bắt buộc.

Một lần nữa, đừng quá sa lầy vào bùn xung quanh hành vi cụ thể của máy tính. Tập trung vào *bộ nhớ* của closure.

Hãy tự mình thử bài tập, sau đó xem giải pháp được đề xuất ở cuối phụ lục này.

## Modules

Bài tập này là chuyển đổi máy tính từ Closure (PHẦN 3) thành một module.

Chúng ta không thêm bất kỳ chức năng bổ sung nào vào máy tính, chỉ thay đổi giao diện của nó. Thay vì gọi một hàm duy nhất `calc(..)`, chúng ta sẽ gọi các phương thức cụ thể trên API công khai cho mỗi "lần nhấn phím" của máy tính của chúng ta. Các đầu ra vẫn giữ nguyên.

Module này nên được thể hiện như một hàm nhà máy module cổ điển gọi là `calculator()`, thay vì một singleton IIFE, để nhiều máy tính có thể được tạo ra nếu muốn.

API công khai nên bao gồm các phương thức sau:

* `number(..)` (đầu vào: ký tự/số "được nhấn")
* `plus()`
* `minus()`
* `mult()`
* `div()`
* `eq()`

Cách sử dụng sẽ trông giống như:

```js
var calc = calculator();

calc.number("4");     // 4
calc.plus();          // +
calc.number("7");     // 7
calc.number("3");     // 3
calc.minus();         // -
calc.number("2");     // 2
calc.eq();            // 75
```

`formatTotal(..)` vẫn giữ nguyên từ bài tập trước đó. Nhưng trình trợ giúp `useCalc(..)` cần được điều chỉnh để làm việc với API module:

```js
function useCalc(calc,keys) {
    var keyMappings = {
        "+": "plus",
        "-": "minus",
        "*": "mult",
        "/": "div",
        "=": "eq"
    };

    return [...keys].reduce(
        function showDisplay(display,key){
            var fn = keyMappings[key] || "number";
            var ret = String( calc[fn](key) );
            return (
                display +
                (
                  (ret != "" && key == "=") ?
                      "=" :
                      ""
                ) +
                ret
            );
        },
        ""
    );
}

useCalc(calc,"4+3=");           // 4+3=7
useCalc(calc,"+9=");            // +9=16
useCalc(calc,"*8=");            // *5=128
useCalc(calc,"7*2*3=");         // 7*2*3=42
useCalc(calc,"1/0=");           // 1/0=ERR
useCalc(calc,"+3=");            // +3=ERR
useCalc(calc,"51=");            // 51
```

Hãy tự mình thử bài tập, sau đó xem giải pháp được đề xuất ở cuối phụ lục này.

Khi bạn làm việc trên bài tập này, cũng hãy dành một chút thời gian xem xét những ưu/nhược điểm của việc thể hiện máy tính như một module trái ngược với cách tiếp cận hàm closure từ bài tập trước.

BONUS: viết ra một vài câu giải thích suy nghĩ của bạn.

BONUS #2: thử chuyển đổi module của bạn sang các định dạng module khác, bao gồm: UMD, CommonJS, và ESM (ES Modules).

## Giải Pháp Đề Xuất

Hy vọng bạn đã thử các bài tập trước khi bạn đọc đến đây. Không gian lận!

Hãy nhớ rằng, mỗi giải pháp được đề xuất chỉ là một trong một loạt các cách khác nhau để tiếp cận các vấn đề. Chúng không phải là "câu trả lời đúng," nhưng chúng minh họa một cách hợp lý để tiếp cận mỗi bài tập.

Lợi ích quan trọng nhất bạn có thể nhận được từ việc đọc các giải pháp được đề xuất này là so sánh chúng với mã của bạn và phân tích lý do tại sao mỗi chúng ta đưa ra các lựa chọn tương tự hoặc khác nhau. Đừng đi quá sâu vào chi tiết nhỏ nhặt; hãy cố gắng tập trung vào chủ đề chính thay vì các chi tiết nhỏ.

### Đề Xuất: Thùng Bi (Buckets of Marbles)

*Bài Tập Thùng Bi* có thể được giải quyết như thế này:

```js
// RED(1)
const howMany = 100;

// Sàng Eratosthenes
function findPrimes(howMany) {
    // BLUE(2)
    var sieve = Array(howMany).fill(true);
    var max = Math.sqrt(howMany);

    for (let i = 2; i < max; i++) {
        // GREEN(3)
        if (sieve[i]) {
            // ORANGE(4)
            let j = Math.pow(i,2);
            for (let k = j; k < howMany; k += i) {
                // PURPLE(5)
                sieve[k] = false;
            }
        }
    }

    return sieve
        .map(function getPrime(flag,prime){
            // PINK(6)
            if (flag) return prime;
            return flag;
        })
        .filter(function onlyPrimes(v){
            // YELLOW(7)
            return !!v;
        })
        .slice(1);
}

findPrimes(howMany);
// [
//    2, 3, 5, 7, 11, 13, 17,
//    19, 23, 29, 31, 37, 41,
//    43, 47, 53, 59, 61, 67,
//    71, 73, 79, 83, 89, 97
// ]
```

### Đề Xuất: Closure (PHẦN 1)

*Bài Tập Closure (PHẦN 1)* cho `isPrime(..)` và `factorize(..)`, có thể được giải quyết như thế này:

```js
var isPrime = (function isPrime(v){
    var primes = {};

    return function isPrime(v) {
        if (v in primes) {
            return primes[v];
        }
        if (v <= 3) {
            return (primes[v] = v > 1);
        }
        if (v % 2 == 0 || v % 3 == 0) {
            return (primes[v] = false);
        }
        let vSqrt = Math.sqrt(v);
        for (let i = 5; i <= vSqrt; i += 6) {
            if (v % i == 0 || v % (i + 2) == 0) {
                return (primes[v] = false);
            }
        }
        return (primes[v] = true);
    };
})();

var factorize = (function factorize(v){
    var factors = {};

    return function findFactors(v) {
        if (v in factors) {
            return factors[v];
        }
        if (!isPrime(v)) {
            let i = Math.floor(Math.sqrt(v));
            while (v % i != 0) {
                i--;
            }
            return (factors[v] = [
                ...findFactors(i),
                ...findFactors(v / i)
            ]);
        }
        return (factors[v] = [v]);
    };
})();
```

Các bước chung tôi đã sử dụng cho mỗi tiện ích:

1. Bọc một IIFE để định nghĩa phạm vi cho biến bộ nhớ cache cư trú.

2. Trong cuộc gọi cơ bản, trước tiên hãy kiểm tra bộ nhớ cache, và nếu kết quả đã được biết, hãy trả về.

3. Tại mỗi nơi mà một `return` đang xảy ra ban đầu, gán cho bộ nhớ cache và chỉ trả về kết quả của hoạt động gán đó—đây là một thủ thuật tiết kiệm không gian chủ yếu chỉ để ngắn gọn trong cuốn sách.

Tôi cũng đã đổi tên hàm bên trong từ `factorize(..)` thành `findFactors(..)`. Điều đó về mặt kỹ thuật không cần thiết, nhưng nó giúp làm rõ hơn hàm nào các cuộc gọi đệ quy gọi.

### Đề Xuất: Closure (PHẦN 2)

*Bài Tập Closure (PHẦN 2)* `toggle(..)` có thể được giải quyết như thế này:

```js
function toggle(...vals) {
    var unset = {};
    var cur = unset;

    return function next(){
        // lưu giá trị trước đó trở lại
        // cuối danh sách
        if (cur != unset) {
            vals.push(cur);
        }
        cur = vals.shift();
        return cur;
    };
}

var hello = toggle("hello");
var onOff = toggle("on","off");
var speed = toggle("slow","medium","fast");

hello();      // "hello"
hello();      // "hello"

onOff();      // "on"
onOff();      // "off"
onOff();      // "on"

speed();      // "slow"
speed();      // "medium"
speed();      // "fast"
speed();      // "slow"
```

### Đề Xuất: Closure (PHẦN 3)

*Bài Tập Closure (PHẦN 3)* `calculator()` có thể được giải quyết như thế này:

```js
// từ trước đó:
//
// function useCalc(..) { .. }
// function formatTotal(..) { .. }

function calculator() {
    var currentTotal = 0;
    var currentVal = "";
    var currentOper = "=";

    return pressKey;

    // ********************

    function pressKey(key){
        // phím số?
        if (/\d/.test(key)) {
            currentVal += key;
            return key;
        }
        // phím toán tử?
        else if (/[+*/-]/.test(key)) {
            // nhiều phép toán trong một chuỗi?
            if (
                currentOper != "=" &&
                currentVal != ""
            ) {
                // ngụ ý nhấn phím '='
                pressKey("=");
            }
            else if (currentVal != "") {
                currentTotal = Number(currentVal);
            }
            currentOper = key;
            currentVal = "";
            return key;
        }
        // phím =?
        else if (
            key == "=" &&
            currentOper != "="
        ) {
            currentTotal = op(
                currentTotal,
                currentOper,
                Number(currentVal)
            );
            currentOper = "=";
            currentVal = "";
            return formatTotal(currentTotal);
        }
        return "";
    };

    function op(val1,oper,val2) {
        var ops = {
            // LƯU Ý: sử dụng hàm mũi tên
            // chỉ để ngắn gọn trong cuốn sách
            "+": (v1,v2) => v1 + v2,
            "-": (v1,v2) => v1 - v2,
            "*": (v1,v2) => v1 * v2,
            "/": (v1,v2) => v1 / v2
        };
        return ops[oper](val1,val2);
    }
}

var calc = calculator();

useCalc(calc,"4+3=");           // 4+3=7
useCalc(calc,"+9=");            // +9=16
useCalc(calc,"*8=");            // *5=128
useCalc(calc,"7*2*3=");         // 7*2*3=42
useCalc(calc,"1/0=");           // 1/0=ERR
useCalc(calc,"+3=");            // +3=ERR
useCalc(calc,"51=");            // 51
```

| LƯU Ý: |
| :--- |
| Hãy nhớ: bài tập này là về closure. Đừng tập trung quá nhiều vào cơ chế thực tế của một máy tính, mà thay vào đó là liệu bạn có đang *nhớ* trạng thái máy tính đúng cách qua các cuộc gọi hàm hay không. |

### Đề Xuất: Modules

*Bài Tập Modules* `calculator()` có thể được giải quyết như thế này:

```js
// từ trước đó:
//
// function useCalc(..) { .. }
// function formatTotal(..) { .. }

function calculator() {
    var currentTotal = 0;
    var currentVal = "";
    var currentOper = "=";

    var publicAPI = {
        number,
        eq,
        plus() { return operator("+"); },
        minus() { return operator("-"); },
        mult() { return operator("*"); },
        div() { return operator("/"); }
    };

    return publicAPI;

    // ********************

    function number(key) {
        // phím số?
        if (/\d/.test(key)) {
            currentVal += key;
            return key;
        }
    }

    function eq() {
        // phím =?
        if (currentOper != "=") {
            currentTotal = op(
                currentTotal,
                currentOper,
                Number(currentVal)
            );
            currentOper = "=";
            currentVal = "";
            return formatTotal(currentTotal);
        }
        return "";
    }

    function operator(key) {
        // nhiều phép toán trong một chuỗi?
        if (
            currentOper != "=" &&
            currentVal != ""
        ) {
            // ngụ ý nhấn phím '='
            eq();
        }
        else if (currentVal != "") {
            currentTotal = Number(currentVal);
        }
        currentOper = key;
        currentVal = "";
        return key;
    }

    function op(val1,oper,val2) {
        var ops = {
            // LƯU Ý: sử dụng hàm mũi tên
            // chỉ để ngắn gọn trong cuốn sách
            "+": (v1,v2) => v1 + v2,
            "-": (v1,v2) => v1 - v2,
            "*": (v1,v2) => v1 * v2,
            "/": (v1,v2) => v1 / v2
        };
        return ops[oper](val1,val2);
    }
}

var calc = calculator();

useCalc(calc,"4+3=");           // 4+3=7
useCalc(calc,"+9=");            // +9=16
useCalc(calc,"*8=");            // *5=128
useCalc(calc,"7*2*3=");         // 7*2*3=42
useCalc(calc,"1/0=");           // 1/0=ERR
useCalc(calc,"+3=");            // +3=ERR
useCalc(calc,"51=");            // 51
```

Đó là tất cả cho cuốn sách này, chúc mừng thành tích của bạn! Khi bạn đã sẵn sàng, hãy chuyển sang Cuốn 3, *Đối Tượng & Lớp* (Objects & Classes).

[^MathJSisPrime]: *Math.js: isPrime(..)*, https://github.com/josdejong/mathjs/blob/develop/src/function/utils/isPrime.js, 3 March 2020.
