# You Don't Know JS Yet: Bắt đầu - Ấn bản thứ 2
# Chương 1: JavaScript *Là* Gì?

Bạn chưa biết JS đâu. Tôi cũng vậy, không hoàn toàn. Không ai trong chúng ta biết hết cả. Nhưng tất cả chúng ta đều có thể bắt đầu tìm hiểu JS tốt hơn.

Trong chương đầu tiên của cuốn sách đầu tiên thuộc bộ *You Don't Know JS Yet* (YDKJSY) này, chúng ta sẽ dành chút thời gian để xây dựng nền tảng để tiến về phía trước. Chúng ta cần bắt đầu bằng cách bao quát một loạt các chi tiết dọn dẹp nền tảng quan trọng, làm sáng tỏ một số huyền thoại và quan niệm sai lầm về ngôn ngữ thực sự là gì (và không phải là gì!).

Đây là cái nhìn sâu sắc có giá trị về bản sắc và quy trình tổ chức cũng như duy trì JS; tất cả các nhà phát triển JS nên hiểu điều đó. Nếu bạn muốn tìm hiểu JS, đây là cách để *bắt đầu* thực hiện những bước đầu tiên trong hành trình đó.

## Về cuốn sách này

Tôi nhấn mạnh từ hành trình bởi vì *biết JS* không phải là một điểm đến, nó là một hướng đi. Bất kể bạn dành bao nhiêu thời gian cho ngôn ngữ này, bạn sẽ luôn có thể tìm thấy điều gì đó khác để học và hiểu rõ hơn một chút. Vì vậy, đừng coi cuốn sách này là thứ gì đó để lướt qua nhanh chóng nhằm đạt được thành tích. Thay vào đó, sự kiên nhẫn và bền bỉ là tốt nhất khi bạn thực hiện những bước đầu tiên này.

Tiếp theo chương nền tảng này, phần còn lại của cuốn sách đưa ra một bản đồ cấp cao về những gì bạn sẽ tìm thấy khi đào sâu và nghiên cứu JS với các cuốn sách YDKJSY.

Cụ thể, Chương 4 xác định ba trụ cột chính mà ngôn ngữ JS được tổ chức xung quanh: phạm vi/closure, prototype/đối tượng, và kiểu/ép kiểu. JS là một ngôn ngữ rộng lớn và tinh vi, với nhiều tính năng và khả năng. Nhưng tất cả JS đều được thành lập dựa trên ba trụ cột nền tảng này.

Hãy nhớ rằng mặc dù cuốn sách này có tiêu đề "Bắt đầu", nhưng nó **không nhằm mục đích là một cuốn sách cho người mới bắt đầu/nhập môn**. Công việc chính của cuốn sách này là giúp bạn sẵn sàng nghiên cứu sâu về JS trong suốt phần còn lại của bộ sách; nó được viết với giả định rằng bạn đã quen thuộc với JS qua ít nhất vài tháng kinh nghiệm trước khi tiếp tục với YDKJSY. Vì vậy, để tận dụng tối đa *Bắt đầu*, hãy chắc chắn rằng bạn dành nhiều thời gian viết mã JS để tích lũy kinh nghiệm.

Ngay cả khi bạn đã viết rất nhiều JS trước đây, cuốn sách này không nên bị đọc lướt hoặc bỏ qua; hãy dành thời gian để xử lý đầy đủ tài liệu ở đây. **Một khởi đầu tốt luôn phụ thuộc vào bước đầu tiên vững chắc.**

## Cái tên đó là sao?

Cái tên JavaScript có lẽ là tên ngôn ngữ lập trình bị nhầm lẫn và hiểu lầm nhiều nhất.

Ngôn ngữ này có liên quan đến Java không? Nó có phải chỉ là dạng script cho Java không? Nó có phải chỉ để viết script chứ không phải chương trình thực sự không?

Sự thật là, cái tên JavaScript là một sản phẩm của những trò lừa gạt tiếp thị. Khi Brendan Eich lần đầu tiên hình thành ngôn ngữ này, ông đặt tên mã cho nó là Mocha. Trong nội bộ Netscape, thương hiệu LiveScript đã được sử dụng. Nhưng khi đến lúc công khai đặt tên cho ngôn ngữ, "JavaScript" đã giành chiến thắng.

Tại sao? Bởi vì ngôn ngữ này ban đầu được thiết kế để thu hút đối tượng chủ yếu là các lập trình viên Java, và bởi vì từ "script" rất phổ biến vào thời điểm đó để chỉ các chương trình nhẹ. Những "script" nhẹ này sẽ là những thứ đầu tiên được nhúng vào bên trong các trang trên thứ mới mẻ gọi là web!

Nói cách khác, JavaScript là một mánh khóe tiếp thị để cố gắng định vị ngôn ngữ này như một sự thay thế dễ chịu cho việc viết Java nặng nề và nổi tiếng hơn vào thời điểm đó. Nó cũng có thể dễ dàng được gọi là "WebJava", về vấn đề đó.

Có một số điểm tương đồng hời hợt giữa mã của JavaScript và mã của Java. Những điểm tương đồng đó không đặc biệt đến từ sự phát triển chung, mà từ cả hai ngôn ngữ đều nhắm đến các nhà phát triển với những kỳ vọng về cú pháp giả định từ C (và ở một mức độ nào đó, C++).

Ví dụ, chúng ta sử dụng `{` để bắt đầu một khối mã và `}` để kết thúc khối mã đó, giống như C/C++ và Java. Chúng ta cũng sử dụng `;` để ngắt câu lệnh.

Theo một số cách, các mối quan hệ pháp lý còn sâu sắc hơn cả cú pháp. Oracle (thông qua Sun), công ty vẫn sở hữu và điều hành Java, cũng sở hữu nhãn hiệu chính thức cho cái tên "JavaScript" (thông qua Netscape). Nhãn hiệu này hầu như không bao giờ được thực thi, và có khả năng là không thể vào thời điểm này.

Vì những lý do này, một số người đã đề xuất chúng ta sử dụng JS thay vì JavaScript. Đó là một cách viết tắt rất phổ biến, nếu không muốn nói là một ứng cử viên sáng giá cho việc xây dựng thương hiệu ngôn ngữ chính thức. Thật vậy, những cuốn sách này sử dụng JS gần như độc quyền để chỉ ngôn ngữ.

Xa hơn nữa việc tách ngôn ngữ khỏi nhãn hiệu do Oracle sở hữu, tên chính thức của ngôn ngữ được quy định bởi TC39 và được chính thức hóa bởi cơ quan tiêu chuẩn ECMA là **ECMAScript**. Và thực sự, kể từ năm 2016, tên ngôn ngữ chính thức cũng đã được thêm hậu tố là năm sửa đổi; tính đến thời điểm viết bài này, đó là ECMAScript 2019, hoặc viết tắt là ES2019.

Nói cách khác, JavaScript/JS chạy trong trình duyệt của bạn hoặc trong Node.js, là *một* triển khai của tiêu chuẩn ES2019.

| LƯU Ý: |
| :--- |
| Đừng sử dụng các thuật ngữ như "JS6" hoặc "ES8" để chỉ ngôn ngữ. Một số người làm vậy, nhưng những thuật ngữ đó chỉ phục vụ để duy trì sự nhầm lẫn. "ES20xx" hoặc chỉ "JS" là những gì bạn nên tuân theo. |

Cho dù bạn gọi nó là JavaScript, JS, ECMAScript hay ES2019, nó chắc chắn không phải là một biến thể của ngôn ngữ Java!

> "Java đối với JavaScript cũng giống như Ham (thịt giăm bông) đối với Hamster (chuột lang)." --Jeremy Keith, 2009

## Đặc tả ngôn ngữ

Tôi đã đề cập đến TC39, ủy ban chỉ đạo kỹ thuật quản lý JS. Nhiệm vụ chính của họ là quản lý đặc tả chính thức cho ngôn ngữ. Họ họp thường xuyên để bỏ phiếu về bất kỳ thay đổi nào đã được thống nhất, sau đó họ gửi cho ECMA, tổ chức tiêu chuẩn.

Cú pháp và hành vi của JS được định nghĩa trong đặc tả ES.

ES2019 tình cờ là đặc tả/bản sửa đổi được đánh số chính thứ 10 kể từ khi JS ra đời vào năm 1995, vì vậy trong URL chính thức của đặc tả do ECMA lưu trữ, bạn sẽ thấy "10.0":

https://www.ecma-international.org/ecma-262/10.0/

Ủy ban TC39 bao gồm từ 50 đến khoảng 100 người khác nhau từ một bộ phận rộng lớn các công ty đầu tư vào web, chẳng hạn như các nhà sản xuất trình duyệt (Mozilla, Google, Apple) và các nhà sản xuất thiết bị (Samsung, v.v.). Tất cả các thành viên của ủy ban đều là tình nguyện viên, mặc dù nhiều người trong số họ là nhân viên của các công ty này và do đó có thể nhận được một phần thù lao cho nhiệm vụ của họ trong ủy ban.

TC39 thường họp khoảng hai tháng một lần, thường là khoảng ba ngày, để xem xét công việc do các thành viên thực hiện kể từ cuộc họp trước, thảo luận về các vấn đề và bỏ phiếu cho các đề xuất. Địa điểm họp luân phiên giữa các công ty thành viên sẵn sàng đăng cai.

Tất cả các đề xuất của TC39 đều tiến triển qua quy trình năm giai đoạn—tất nhiên, vì chúng ta là lập trình viên, nên nó bắt đầu từ 0!—Giai đoạn 0 đến Giai đoạn 4. Bạn có thể đọc thêm về quy trình Giai đoạn tại đây: https://tc39.es/process-document/

Giai đoạn 0 có nghĩa đại khái là, ai đó trong TC39 nghĩ rằng đó là một ý tưởng xứng đáng và có kế hoạch ủng hộ và thực hiện nó. Điều đó có nghĩa là rất nhiều ý tưởng mà các thành viên không thuộc TC39 "đề xuất", thông qua các phương tiện không chính thức như mạng xã hội hoặc bài đăng trên blog, thực sự là "tiền giai đoạn 0". Bạn phải có một thành viên TC39 ủng hộ một đề xuất để nó được coi là "Giai đoạn 0" một cách chính thức.

Khi một đề xuất đạt đến trạng thái "Giai đoạn 4", nó đủ điều kiện để được đưa vào bản sửa đổi hàng năm tiếp theo của ngôn ngữ. Có thể mất từ vài tháng đến vài năm để một đề xuất đi qua các giai đoạn này.

Tất cả các đề xuất được quản lý công khai, trên kho lưu trữ Github của TC39: https://github.com/tc39/proposals

Bất kỳ ai, dù có thuộc TC39 hay không, đều được hoan nghênh tham gia vào các cuộc thảo luận công khai này và các quy trình làm việc trên các đề xuất. Tuy nhiên, chỉ các thành viên TC39 mới có thể tham dự các cuộc họp và bỏ phiếu cho các đề xuất và thay đổi. Vì vậy, trên thực tế, tiếng nói của một thành viên TC39 có trọng lượng rất lớn trong việc JS sẽ đi về đâu.

Trái ngược với một số huyền thoại đã được thiết lập và duy trì một cách đáng thất vọng, *không* có nhiều phiên bản JavaScript trong tự nhiên. Chỉ có **một JS**, tiêu chuẩn chính thức được duy trì bởi TC39 và ECMA.

Quay trở lại đầu những năm 2000, khi Microsoft duy trì một phiên bản JS được phân nhánh và thiết kế ngược (và không hoàn toàn tương thích) có tên là "JScript", đã có "nhiều phiên bản" JS một cách hợp pháp. Nhưng những ngày đó đã qua lâu rồi. Thật lỗi thời và không chính xác khi đưa ra những tuyên bố như vậy về JS ngày nay.

Tất cả các trình duyệt chính và nhà sản xuất thiết bị đã cam kết giữ cho việc triển khai JS của họ tuân thủ đặc tả trung tâm này. Tất nhiên, các engine triển khai các tính năng vào những thời điểm khác nhau. Nhưng không bao giờ có trường hợp engine v8 (engine JS của Chrome) triển khai một tính năng được chỉ định khác hoặc không tương thích so với engine SpiderMonkey (engine JS của Mozilla).

Điều đó có nghĩa là bạn có thể học **một JS**, và dựa vào cùng một JS đó ở mọi nơi.

### Web thống trị mọi thứ về (JS)

Trong khi mảng các môi trường chạy JS không ngừng mở rộng (từ trình duyệt, đến máy chủ (Node.js), đến robot, đến bóng đèn, đến...), môi trường duy nhất thống trị JS là web. Nói cách khác, cách JS được triển khai cho các trình duyệt web, về mặt thực tế, là thực tế duy nhất quan trọng.

Về phần lớn, JS được định nghĩa trong đặc tả và JS chạy trong các engine JS dựa trên trình duyệt là giống nhau. Nhưng có một số khác biệt cần được xem xét.

Đôi khi đặc tả JS sẽ ra lệnh cho một số hành vi mới hoặc tinh chỉnh, nhưng điều đó sẽ không khớp chính xác với cách nó hoạt động trong các engine JS dựa trên trình duyệt. Sự không khớp như vậy mang tính lịch sử: Các engine JS đã có hơn 20 năm hành vi có thể quan sát được xung quanh các trường hợp góc của các tính năng đã được nội dung web dựa vào. Như vậy, đôi khi các engine JS sẽ từ chối tuân thủ thay đổi do đặc tả quy định vì nó sẽ phá vỡ nội dung web đó.

Trong những trường hợp này, TC39 thường sẽ quay lại và đơn giản là chọn tuân thủ đặc tả theo thực tế của web. Ví dụ: TC39 đã lên kế hoạch thêm phương thức `contains(..)` cho Mảng, nhưng người ta thấy rằng tên này xung đột với các framework JS cũ vẫn đang được sử dụng trên một số trang web, vì vậy họ đã đổi tên thành `includes(..)` không xung đột. Điều tương tự cũng xảy ra với một *cuộc khủng hoảng cộng đồng* JS hài hước/bi thảm được đặt tên là "smooshgate", nơi phương thức `flatten(..)` đã lên kế hoạch cuối cùng được đổi tên thành `flat(..)`.

Nhưng thỉnh thoảng, TC39 sẽ quyết định đặc tả nên giữ vững lập trường về một số điểm ngay cả khi không có khả năng các engine JS dựa trên trình duyệt sẽ tuân thủ.

Giải pháp? Phụ lục B, "Các tính năng ECMAScript bổ sung cho trình duyệt Web".[^specApB] Đặc tả JS bao gồm phụ lục này để chi tiết hóa mọi sự không khớp đã biết giữa đặc tả JS chính thức và thực tế của JS trên web. Nói cách khác, đây là những ngoại lệ được phép *chỉ* đối với JS web; các môi trường JS khác phải tuân thủ đúng luật.

Phần B.1 và B.2 bao gồm các *bổ sung* cho JS (cú pháp và API) mà JS web bao gồm, một lần nữa vì lý do lịch sử, nhưng TC39 không có kế hoạch quy định chính thức trong cốt lõi của JS. Các ví dụ bao gồm các literal bát phân có tiền tố `0`, các tiện ích `escape(..)` / `unescape(..)` toàn cục, các "trình trợ giúp" Chuỗi như `anchor(..)` và `blink()`, và phương thức RegExp `compile(..)`.

Phần B.3 bao gồm một số xung đột trong đó mã có thể chạy trong cả engine JS web và không phải web, nhưng hành vi *có thể* khác biệt rõ rệt, dẫn đến kết quả khác nhau. Hầu hết các thay đổi được liệt kê liên quan đến các tình huống được gắn nhãn là lỗi sớm khi mã đang chạy ở chế độ nghiêm ngặt (strict mode).

Các *gotcha* (bẫy) trong Phụ lục B không thường xuyên gặp phải, nhưng vẫn là một ý tưởng hay để tránh các cấu trúc này để an toàn trong tương lai. Bất cứ khi nào có thể, hãy tuân thủ đặc tả JS và đừng dựa vào hành vi chỉ áp dụng trong một số môi trường engine JS nhất định.

### Không phải tất cả đều là (Web) JS...

Đoạn mã này có phải là một chương trình JS không?

```js
alert("Hello, JS!");
```

Tùy thuộc vào cách bạn nhìn nhận mọi thứ. Hàm `alert(..)` hiển thị ở đây không được bao gồm trong đặc tả JS, nhưng nó *có* trong tất cả các môi trường JS web. Tuy nhiên, bạn sẽ không tìm thấy nó trong Phụ lục B, vậy chuyện gì đang xảy ra?

Nhiều môi trường JS khác nhau (như engine JS trình duyệt, Node.js, v.v.) thêm các API vào phạm vi toàn cục của các chương trình JS của bạn, cung cấp cho bạn các khả năng cụ thể của môi trường, như khả năng bật lên một hộp kiểu cảnh báo trong trình duyệt của người dùng.

Trên thực tế, một loạt các API trông giống JS, như `fetch(..)`, `getCurrentLocation(..)`, và `getUserMedia(..)`, đều là các API web trông giống như JS. Trong Node.js, chúng ta có thể truy cập hàng trăm phương thức API từ các mô-đun tích hợp khác nhau, như `fs.write(..)`.

Một ví dụ phổ biến khác là `console.log(..)` (và tất cả các phương thức `console.*` khác!). Những thứ này không được quy định trong JS, nhưng vì tiện ích phổ quát của chúng, chúng được định nghĩa bởi hầu hết mọi môi trường JS, theo một sự đồng thuận đã được thống nhất đại khái.

Vì vậy, `alert(..)` và `console.log(..)` không được định nghĩa bởi JS. Nhưng chúng *trông* giống như JS. Chúng là các hàm và phương thức đối tượng và chúng tuân theo các quy tắc cú pháp JS. Các hành vi đằng sau chúng được kiểm soát bởi môi trường chạy engine JS, nhưng trên bề mặt, chúng chắc chắn phải tuân thủ JS để có thể chơi trong sân chơi JS.

Hầu hết các khác biệt giữa các trình duyệt mà mọi người phàn nàn với các tuyên bố "JS quá không nhất quán!" thực sự là do sự khác biệt trong cách các hành vi môi trường đó hoạt động, không phải ở cách bản thân JS hoạt động.

Vì vậy, một lệnh gọi `alert(..)` *là* JS, nhưng bản thân `alert` thực sự chỉ là một vị khách, không phải là một phần của đặc tả JS chính thức.

### Không phải lúc nào cũng là JS

Sử dụng console/REPL (Read-Evaluate-Print-Loop) trong Công cụ dành cho nhà phát triển của trình duyệt (hoặc Node) thoạt nhìn có vẻ giống như một môi trường JS khá đơn giản. Nhưng thực sự không phải vậy.

Công cụ dành cho nhà phát triển là... công cụ dành cho nhà phát triển. Mục đích chính của chúng là làm cho cuộc sống của các nhà phát triển dễ dàng hơn. Chúng ưu tiên DX (Trải nghiệm nhà phát triển). *Không* phải là mục tiêu của các công cụ như vậy để phản ánh chính xác và thuần túy tất cả các sắc thái của hành vi JS theo đặc tả nghiêm ngặt. Như vậy, có nhiều điều kỳ quặc có thể đóng vai trò là "gotcha" nếu bạn coi console là một môi trường JS *thuần túy*.

Sự tiện lợi này là một điều tốt, nhân tiện! Tôi rất vui vì Công cụ dành cho nhà phát triển làm cho cuộc sống của các nhà phát triển dễ dàng hơn! Tôi rất vui vì chúng ta có những nét hấp dẫn UX tốt như tự động hoàn thành các biến/thuộc tính, v.v. Tôi chỉ chỉ ra rằng chúng ta không thể và không nên mong đợi các công cụ như vậy *luôn* tuân thủ nghiêm ngặt cách các chương trình JS được xử lý, bởi vì đó không phải là mục đích của các công cụ này.

Vì các công cụ như vậy khác nhau về hành vi từ trình duyệt này sang trình duyệt khác và vì chúng thay đổi (đôi khi khá thường xuyên), tôi sẽ không "hardcode" bất kỳ chi tiết cụ thể nào vào văn bản này, do đó đảm bảo văn bản cuốn sách này nhanh chóng bị lỗi thời.

Nhưng tôi sẽ chỉ gợi ý một số ví dụ về những điều kỳ quặc đã đúng tại các thời điểm khác nhau trong các môi trường console JS khác nhau, để củng cố quan điểm của tôi về việc không giả định hành vi JS gốc trong khi sử dụng chúng:

* Liệu một khai báo `var` hoặc `function` trong "phạm vi toàn cục" cấp cao nhất của console có thực sự tạo ra một biến toàn cục thực sự (và thuộc tính `window` được phản chiếu, và ngược lại!) hay không.

* Điều gì xảy ra với nhiều khai báo `let` và `const` trong "phạm vi toàn cục" cấp cao nhất.

* Liệu `"use strict";` trên một dòng nhập (nhấn `<enter>` sau đó) có kích hoạt chế độ nghiêm ngặt cho phần còn lại của phiên console đó hay không, theo cách nó sẽ làm trên dòng đầu tiên của tệp .js, cũng như liệu bạn có thể sử dụng `"use strict";` ngoài "dòng đầu tiên" mà vẫn bật chế độ nghiêm ngặt cho phiên đó hay không.

* Cách liên kết mặc định `this` ở chế độ không nghiêm ngặt hoạt động đối với các lệnh gọi hàm và liệu "đối tượng toàn cục" được sử dụng có chứa các biến toàn cục dự kiến hay không.

* Cách hoisting (xem Quyển 2, *Phạm vi & Closures*) hoạt động trên nhiều dòng nhập.

* ...một số cái khác

Console của nhà phát triển không cố gắng giả vờ là một trình biên dịch JS xử lý mã đã nhập của bạn giống hệt như cách engine JS xử lý tệp .js. Nó đang cố gắng giúp bạn dễ dàng nhập nhanh một vài dòng mã và xem kết quả ngay lập tức. Đây là những trường hợp sử dụng hoàn toàn khác nhau, và như vậy, thật vô lý khi mong đợi một công cụ xử lý cả hai như nhau.

Đừng tin vào hành vi bạn thấy trong console của nhà phát triển là đại diện cho ngữ nghĩa JS *chính xác* đến từng chữ; để làm điều đó, hãy đọc đặc tả. Thay vào đó, hãy nghĩ về console như một môi trường "thân thiện với JS". Điều đó hữu ích theo cách riêng của nó.

## Nhiều bộ mặt

Thuật ngữ "mô hình" (paradigm) trong bối cảnh ngôn ngữ lập trình đề cập đến một tư duy và cách tiếp cận rộng lớn (gần như phổ quát) để cấu trúc mã. Trong một mô hình, có vô số biến thể về phong cách và hình thức phân biệt các chương trình, bao gồm vô số thư viện và framework khác nhau để lại dấu ấn độc đáo của chúng trên bất kỳ mã nào.

Nhưng bất kể phong cách cá nhân của một chương trình có thể là gì, sự phân chia bức tranh lớn xung quanh các mô hình hầu như luôn hiển nhiên ngay từ cái nhìn đầu tiên về bất kỳ chương trình nào.

Các danh mục mã cấp mô hình điển hình bao gồm thủ tục, hướng đối tượng (OO/lớp) và hàm (FP):

* Phong cách thủ tục tổ chức mã theo tiến trình tuyến tính, từ trên xuống dưới thông qua một tập hợp các hoạt động được xác định trước, thường được thu thập cùng nhau trong các đơn vị liên quan được gọi là thủ tục.

* Phong cách OO tổ chức mã bằng cách thu thập logic và dữ liệu lại với nhau thành các đơn vị được gọi là lớp.

* Phong cách FP tổ chức mã thành các hàm (tính toán thuần túy trái ngược với thủ tục) và sự thích ứng của các hàm đó dưới dạng giá trị.

Các mô hình không đúng cũng không sai. Chúng là những định hướng hướng dẫn và định hình cách các lập trình viên tiếp cận vấn đề và giải pháp, cách họ cấu trúc và duy trì mã của mình.

Một số ngôn ngữ nghiêng hẳn về một mô hình—C là thủ tục, Java/C++ gần như hoàn toàn hướng lớp và Haskell là FP từ đầu đến cuối.

Nhưng nhiều ngôn ngữ cũng hỗ trợ các mẫu mã có thể đến từ, và thậm chí trộn lẫn và kết hợp từ, các mô hình khác nhau. Cái gọi là "ngôn ngữ đa mô hình" cung cấp sự linh hoạt tối đa. Trong một số trường hợp, một chương trình duy nhất thậm chí có thể có hai hoặc nhiều biểu hiện của các mô hình này nằm cạnh nhau.

JavaScript chắc chắn là một ngôn ngữ đa mô hình. Bạn có thể viết mã theo phong cách thủ tục, hướng lớp hoặc FP và bạn có thể đưa ra những quyết định đó trên cơ sở từng dòng thay vì bị buộc phải lựa chọn tất cả hoặc không có gì.

## Tương thích ngược & xuôi

Một trong những nguyên tắc nền tảng nhất hướng dẫn JavaScript là bảo tồn *tương thích ngược*. Nhiều người nhầm lẫn về ý nghĩa của thuật ngữ này và thường nhầm lẫn nó với một thuật ngữ liên quan nhưng khác biệt: *tương thích xuôi*.

Hãy làm rõ vấn đề này.

Tương thích ngược có nghĩa là một khi thứ gì đó được chấp nhận là JS hợp lệ, sẽ không có thay đổi nào trong tương lai đối với ngôn ngữ khiến mã đó trở thành JS không hợp lệ. Mã được viết vào năm 1995—dù sơ khai hay hạn chế đến đâu!—vẫn sẽ hoạt động cho đến ngày nay. Như các thành viên TC39 thường tuyên bố, "chúng tôi không phá vỡ web!"

Ý tưởng là các nhà phát triển JS có thể viết mã với sự tự tin rằng mã của họ sẽ không ngừng hoạt động một cách khó lường vì bản cập nhật trình duyệt được phát hành. Điều này làm cho quyết định chọn JS cho một chương trình trở thành một khoản đầu tư khôn ngoan và an toàn hơn, trong nhiều năm tới trong tương lai.

"Sự đảm bảo" đó không phải là chuyện nhỏ. Việc duy trì khả năng tương thích ngược, kéo dài suốt gần 25 năm lịch sử của ngôn ngữ, tạo ra gánh nặng to lớn và hàng loạt thách thức độc đáo. Bạn sẽ khó tìm thấy nhiều ví dụ khác trong máy tính về cam kết tương thích ngược như vậy.

Chi phí của việc tuân thủ nguyên tắc này không nên bị gạt bỏ một cách tùy tiện. Nó nhất thiết tạo ra một rào cản rất cao để bao gồm việc thay đổi hoặc mở rộng ngôn ngữ; bất kỳ quyết định nào cũng trở nên vĩnh viễn, sai lầm và tất cả. Một khi nó có trong JS, nó không thể bị loại bỏ vì nó có thể phá vỡ các chương trình, ngay cả khi chúng ta thực sự, thực sự muốn loại bỏ nó!

Có một số ngoại lệ nhỏ đối với quy tắc này. JS đã có một số thay đổi không tương thích ngược, nhưng TC39 cực kỳ thận trọng khi làm như vậy. Họ nghiên cứu mã hiện có trên web (thông qua thu thập dữ liệu trình duyệt) để ước tính tác động của sự đổ vỡ như vậy và các trình duyệt cuối cùng quyết định và bỏ phiếu xem liệu họ có sẵn sàng chịu sự chỉ trích từ người dùng cho một sự đổ vỡ quy mô rất nhỏ so với lợi ích của việc sửa chữa hoặc cải thiện một số khía cạnh của ngôn ngữ cho nhiều trang web (và người dùng) hơn hay không.

Những loại thay đổi này rất hiếm và hầu như luôn nằm trong các trường hợp sử dụng góc cạnh khó có thể quan sát thấy sự đổ vỡ trong nhiều trang web.

So sánh *tương thích ngược* với đối tác của nó, *tương thích xuôi*. Tương thích xuôi có nghĩa là việc bao gồm một bổ sung mới cho ngôn ngữ trong một chương trình sẽ không khiến chương trình đó bị hỏng nếu nó được chạy trong một engine JS cũ hơn. **JS không tương thích xuôi**, bất chấp nhiều người mong muốn như vậy, và thậm chí tin tưởng sai lầm vào huyền thoại rằng nó là như vậy.

HTML và CSS, ngược lại, tương thích xuôi nhưng không tương thích ngược. Nếu bạn đào bới một số HTML hoặc CSS được viết lại vào năm 1995, hoàn toàn có thể nó sẽ không hoạt động (hoặc hoạt động giống như vậy) ngày nay. Nhưng, nếu bạn sử dụng một tính năng mới từ năm 2019 trong trình duyệt từ năm 2010, trang sẽ không bị "hỏng" -- CSS/HTML không được công nhận sẽ bị bỏ qua, trong khi phần còn lại của CSS/HTML sẽ được xử lý tương ứng.

Có vẻ như mong muốn khả năng tương thích xuôi được đưa vào thiết kế ngôn ngữ lập trình, nhưng nói chung là không thực tế để làm như vậy. Đánh dấu (HTML) hoặc kiểu dáng (CSS) có bản chất khai báo, vì vậy việc "bỏ qua" các khai báo không được công nhận dễ dàng hơn nhiều với tác động tối thiểu đến các khai báo được công nhận khác.

Nhưng sự hỗn loạn và không xác định sẽ xảy ra nếu một engine ngôn ngữ lập trình chọn lọc bỏ qua các câu lệnh (hoặc thậm chí các biểu thức!) mà nó không hiểu, vì không thể đảm bảo rằng một phần tiếp theo của chương trình không mong đợi phần bị bỏ qua đã được xử lý.

Mặc dù JS không, và không thể, tương thích xuôi, nhưng điều quan trọng là phải nhận ra khả năng tương thích ngược của JS, bao gồm những lợi ích lâu dài cho web và những hạn chế cũng như khó khăn mà nó đặt ra cho JS như một kết quả.

### Nhảy qua các khoảng trống

Vì JS không tương thích xuôi, điều đó có nghĩa là luôn có khả năng xảy ra khoảng cách giữa mã mà bạn có thể viết là JS hợp lệ và engine cũ nhất mà trang web hoặc ứng dụng của bạn cần hỗ trợ. Nếu bạn chạy một chương trình sử dụng tính năng ES2019 trong một engine từ năm 2016, bạn rất có thể sẽ thấy chương trình bị hỏng và gặp sự cố.

Nếu tính năng này là một cú pháp mới, chương trình nói chung sẽ hoàn toàn không biên dịch và chạy được, thường ném ra lỗi cú pháp. Nếu tính năng này là một API (chẳng hạn như `Object.is(..)` của ES6), chương trình có thể chạy đến một điểm nhưng sau đó ném ra một ngoại lệ runtime và dừng lại khi gặp tham chiếu đến API không xác định.

Điều này có nghĩa là các nhà phát triển JS nên luôn tụt hậu so với tốc độ tiến bộ, chỉ sử dụng mã nằm ở rìa sau của các môi trường engine JS cũ nhất mà họ cần hỗ trợ? Không!

Nhưng điều đó có nghĩa là các nhà phát triển JS cần đặc biệt quan tâm để giải quyết khoảng cách này.

Đối với cú pháp mới và không tương thích, giải pháp là transpiling (chuyển đổi mã). Transpiling là một thuật ngữ được phát minh bởi cộng đồng và có phần gượng ép để mô tả việc sử dụng một công cụ để chuyển đổi mã nguồn của một chương trình từ dạng này sang dạng khác (nhưng vẫn là mã nguồn văn bản). Thông thường, các vấn đề tương thích xuôi liên quan đến cú pháp được giải quyết bằng cách sử dụng một transpiler (phổ biến nhất là Babel (https://babeljs.io)) để chuyển đổi từ phiên bản cú pháp JS mới hơn đó sang một cú pháp cũ hơn tương đương.

Ví dụ, một nhà phát triển có thể viết một đoạn mã như:

```js
if (something) {
    let x = 3;
    console.log(x);
}
else {
    let x = 4;
    console.log(x);
}
```

Đây là cách mã sẽ trông như thế nào trong cây mã nguồn cho ứng dụng đó. Nhưng khi tạo (các) tệp để triển khai lên trang web công cộng, transpiler Babel có thể chuyển đổi mã đó trông giống như thế này:

```js
var x$0, x$1;
if (something) {
    x$0 = 3;
    console.log(x$0);
}
else {
    x$1 = 4;
    console.log(x$1);
}
```

Đoạn mã gốc dựa vào `let` để tạo các biến `x` có phạm vi khối trong cả hai mệnh đề `if` và `else` mà không can thiệp lẫn nhau. Một chương trình tương đương (với việc làm lại tối thiểu) mà Babel có thể tạo ra chỉ cần chọn đặt tên cho hai biến khác nhau với tên duy nhất, tạo ra cùng một kết quả không can thiệp.

| LƯU Ý: |
| :--- |
| Từ khóa `let` đã được thêm vào trong ES6 (vào năm 2015). Ví dụ trước về transpiling sẽ chỉ cần áp dụng nếu một ứng dụng cần chạy trong môi trường JS hỗ trợ trước ES6. Ví dụ ở đây chỉ để đơn giản hóa minh họa. Khi ES6 còn mới, nhu cầu transpilation như vậy khá phổ biến, nhưng vào năm 2020, việc cần hỗ trợ các môi trường trước ES6 ít phổ biến hơn nhiều. Do đó, "mục tiêu" được sử dụng cho transpilation là một cửa sổ trượt chỉ dịch chuyển lên trên khi các quyết định được đưa ra cho một trang web/ứng dụng ngừng hỗ trợ một số trình duyệt/engine cũ. |

Bạn có thể tự hỏi: tại sao phải rắc rối sử dụng một công cụ để chuyển đổi từ phiên bản cú pháp mới hơn sang phiên bản cũ hơn? Chúng ta không thể chỉ viết hai biến và bỏ qua việc sử dụng từ khóa `let` sao? Lý do là, các nhà phát triển được khuyến khích mạnh mẽ sử dụng phiên bản JS mới nhất để mã của họ sạch sẽ và truyền đạt ý tưởng hiệu quả nhất.

Các nhà phát triển nên tập trung vào việc viết các dạng cú pháp mới, sạch sẽ và để các công cụ lo việc tạo ra một phiên bản tương thích xuôi của mã đó phù hợp để triển khai và chạy trên các môi trường engine JS được hỗ trợ cũ nhất.

### Lấp đầy các khoảng trống

Nếu vấn đề tương thích xuôi không liên quan đến cú pháp mới, mà là do thiếu phương thức API chỉ mới được thêm vào gần đây, giải pháp phổ biến nhất là cung cấp một định nghĩa cho phương thức API còn thiếu đó, thay thế và hoạt động như thể môi trường cũ hơn đã có nó được định nghĩa nguyên bản. Mẫu này được gọi là polyfill (hay còn gọi là "shim").

Hãy xem xét mã này:

```js
// getSomeRecords() trả về cho chúng ta một promise cho một số
// dữ liệu nó sẽ tìm nạp
var pr = getSomeRecords();

// hiển thị spinner UI trong khi chúng ta lấy dữ liệu
startSpinner();

pr
.then(renderRecords)   // render nếu thành công
.catch(showError)      // hiển thị lỗi nếu không
.finally(hideSpinner)  // luôn ẩn spinner
```

Mã này sử dụng một tính năng ES2019, phương thức `finally(..)` trên prototype promise. Nếu mã này được sử dụng trong môi trường trước ES2019, phương thức `finally(..)` sẽ không tồn tại và lỗi sẽ xảy ra.

Một polyfill cho `finally(..)` trong môi trường trước ES2019 có thể trông như thế này:

```js
if (!Promise.prototype.finally) {
    Promise.prototype.finally = function f(fn){
        return this.then(
            function t(v){
                return Promise.resolve( fn() )
                    .then(function t(){
                        return v;
                    });
            },
            function c(e){
                return Promise.resolve( fn() )
                    .then(function t(){
                        throw e;
                    });
            }
        );
    };
}
```

| CẢNH BÁO: |
| :--- |
| Đây chỉ là một minh họa đơn giản về một polyfill cơ bản (không hoàn toàn tuân thủ đặc tả) cho `finally(..)`. Đừng sử dụng polyfill này trong mã của bạn; luôn sử dụng một polyfill chính thức, mạnh mẽ bất cứ khi nào có thể, chẳng hạn như bộ sưu tập polyfill/shim trong ES-Shim. |

Câu lệnh `if` bảo vệ định nghĩa polyfill bằng cách ngăn nó chạy trong bất kỳ môi trường nào mà engine JS đã định nghĩa phương thức đó. Trong các môi trường cũ hơn, polyfill được định nghĩa, nhưng trong các môi trường mới hơn, câu lệnh `if` bị bỏ qua một cách lặng lẽ.

Các transpiler như Babel thường phát hiện polyfill nào mã của bạn cần và cung cấp chúng tự động cho bạn. Nhưng đôi khi bạn có thể cần bao gồm/định nghĩa chúng một cách rõ ràng, hoạt động tương tự như đoạn mã chúng ta vừa xem xét.

Luôn viết mã sử dụng các tính năng phù hợp nhất để truyền đạt ý tưởng và ý định của nó một cách hiệu quả. Nói chung, điều này có nghĩa là sử dụng phiên bản JS ổn định gần đây nhất. Tránh tác động tiêu cực đến khả năng đọc của mã bằng cách cố gắng điều chỉnh thủ công cho các khoảng trống cú pháp/API. Đó là những gì các công cụ dùng để làm!

Transpilation và polyfilling là hai kỹ thuật hiệu quả cao để giải quyết khoảng cách đó giữa mã sử dụng các tính năng ổn định mới nhất trong ngôn ngữ và các môi trường cũ mà một trang web hoặc ứng dụng vẫn cần hỗ trợ. Vì JS sẽ không ngừng cải thiện, khoảng cách sẽ không bao giờ biến mất. Cả hai kỹ thuật nên được chấp nhận như một phần tiêu chuẩn của chuỗi sản xuất của mọi dự án JS trong tương lai.

## Có gì trong việc thông dịch?

Một câu hỏi được tranh luận từ lâu đối với mã được viết bằng JS: nó là một script được thông dịch hay một chương trình được biên dịch? Ý kiến đa số dường như cho rằng JS là một ngôn ngữ thông dịch (scripting). Nhưng sự thật phức tạp hơn thế.

Trong phần lớn lịch sử của các ngôn ngữ lập trình, các ngôn ngữ "thông dịch" và ngôn ngữ "scripting" đã bị coi thường là kém hơn so với các đối tác được biên dịch của chúng. Lý do cho sự gay gắt này rất nhiều, bao gồm nhận thức rằng thiếu tối ưu hóa hiệu suất, cũng như không thích một số đặc điểm ngôn ngữ nhất định, chẳng hạn như các ngôn ngữ scripting thường sử dụng kiểu động thay vì các ngôn ngữ kiểu tĩnh "trưởng thành hơn".

Các ngôn ngữ được coi là "biên dịch" thường tạo ra một biểu diễn di động (nhị phân) của chương trình được phân phối để thực thi sau này. Vì chúng ta không thực sự quan sát thấy loại mô hình đó với JS (chúng ta phân phối mã nguồn, không phải dạng nhị phân), nhiều người cho rằng điều đó loại JS khỏi danh mục này. Trên thực tế, mô hình phân phối cho dạng "có thể thực thi" của một chương trình đã trở nên đa dạng hơn đáng kể và cũng ít liên quan hơn trong vài thập kỷ qua; đối với câu hỏi hiện tại, thực sự không còn quan trọng lắm về việc dạng nào của một chương trình được truyền đi.

Những tuyên bố và chỉ trích sai lầm này nên được gạt sang một bên. Lý do thực sự quan trọng để có một bức tranh rõ ràng về việc JS được thông dịch hay biên dịch liên quan đến bản chất của cách xử lý lỗi.

Về mặt lịch sử, các ngôn ngữ script hoặc thông dịch được thực thi theo kiểu từ trên xuống dưới và từng dòng; thường không có bước đầu tiên qua chương trình để xử lý nó trước khi bắt đầu thực thi (xem Hình 1).

<figure>
    <img src="images/fig1.png" width="650" alt="Thông dịch một script để thực thi nó" align="center">
    <figcaption><em>Hình 1: Thực thi Thông dịch/Script</em></figcaption>
    <br><br>
</figure>

Trong các ngôn ngữ script hoặc thông dịch, lỗi ở dòng 5 của chương trình sẽ không được phát hiện cho đến khi các dòng từ 1 đến 4 đã được thực thi. Đáng chú ý, lỗi ở dòng 5 có thể do điều kiện runtime, chẳng hạn như một số biến hoặc giá trị có giá trị không phù hợp cho một thao tác, hoặc có thể do câu lệnh/lệnh bị lỗi trên dòng đó. Tùy thuộc vào ngữ cảnh, việc trì hoãn xử lý lỗi đến dòng xảy ra lỗi có thể là một hiệu ứng mong muốn hoặc không mong muốn.

So sánh điều đó với các ngôn ngữ trải qua bước xử lý (thường được gọi là phân tích cú pháp - parsing) trước khi bất kỳ quá trình thực thi nào xảy ra, như được minh họa trong Hình 2:

<figure>
    <img src="images/fig2.png" width="650" alt="Phân tích cú pháp, biên dịch và thực thi một chương trình" align="center">
    <figcaption><em>Hình 2: Phân tích cú pháp + Biên dịch + Thực thi</em></figcaption>
    <br><br>
</figure>

Trong mô hình xử lý này, một lệnh không hợp lệ (chẳng hạn như cú pháp bị hỏng) trên dòng 5 sẽ bị bắt trong giai đoạn phân tích cú pháp, trước khi bất kỳ quá trình thực thi nào bắt đầu và không có phần nào của chương trình sẽ chạy. Để bắt các lỗi cú pháp (hoặc nói cách khác là "tĩnh"), nhìn chung, tốt hơn là nên biết về chúng trước bất kỳ quá trình thực thi một phần nào bị hủy hoại.

Vậy các ngôn ngữ "được phân tích cú pháp" có điểm gì chung với các ngôn ngữ "được biên dịch"? Đầu tiên, tất cả các ngôn ngữ biên dịch đều được phân tích cú pháp. Vì vậy, một ngôn ngữ được phân tích cú pháp đã đi được một chặng đường dài để được biên dịch rồi. Trong lý thuyết biên dịch cổ điển, bước cuối cùng còn lại sau khi phân tích cú pháp là tạo mã: tạo ra một dạng có thể thực thi.

Khi bất kỳ chương trình nguồn nào đã được phân tích cú pháp đầy đủ, rất phổ biến là quá trình thực thi tiếp theo của nó, dưới một hình thức hoặc kiểu cách nào đó, sẽ bao gồm một bản dịch từ dạng đã phân tích cú pháp của chương trình—thường được gọi là Cây cú pháp trừu tượng (AST)—sang dạng có thể thực thi đó.

Nói cách khác, các ngôn ngữ được phân tích cú pháp thường cũng thực hiện tạo mã trước khi thực thi, vì vậy không quá lời khi nói rằng, về mặt tinh thần, chúng là các ngôn ngữ biên dịch.

Mã nguồn JS được phân tích cú pháp trước khi nó được thực thi. Đặc tả yêu cầu nhiều như vậy, bởi vì nó kêu gọi các "lỗi sớm"—các lỗi được xác định tĩnh trong mã, chẳng hạn như tên tham số trùng lặp—phải được báo cáo trước khi mã bắt đầu thực thi. Những lỗi đó không thể được nhận ra nếu mã không được phân tích cú pháp.

Vì vậy, **JS là một ngôn ngữ được phân tích cú pháp**, nhưng nó có được *biên dịch* không?

Câu trả lời gần với có hơn là không. JS đã phân tích cú pháp được chuyển đổi thành dạng tối ưu hóa (nhị phân), và "mã" đó sau đó được thực thi (Hình 2); engine thường không chuyển trở lại chế độ thực thi từng dòng (như Hình 1) sau khi đã hoàn thành tất cả công việc khó khăn của việc phân tích cú pháp—hầu hết các ngôn ngữ/engine sẽ không làm vậy, vì điều đó sẽ rất kém hiệu quả.

Cụ thể, quá trình "biên dịch" này tạo ra một mã byte nhị phân (đại loại vậy), sau đó được chuyển cho "máy ảo JS" để thực thi. Một số người thích nói rằng VM này đang "thông dịch" mã byte. Nhưng điều đó có nghĩa là Java, và hàng tá ngôn ngữ dựa trên JVM khác, về vấn đề đó, được thông dịch thay vì biên dịch. Tất nhiên, điều đó mâu thuẫn với khẳng định điển hình rằng Java/v.v. là các ngôn ngữ biên dịch.

Thật thú vị, trong khi Java và JavaScript là những ngôn ngữ rất khác nhau, câu hỏi về thông dịch/biên dịch lại khá liên quan chặt chẽ giữa chúng!

Một vấn đề khác là các engine JS có thể sử dụng nhiều lượt xử lý/tối ưu hóa JIT (Just-In-Time) trên mã được tạo (sau khi phân tích cú pháp), một lần nữa có thể được dán nhãn hợp lý là "biên dịch" hoặc "thông dịch" tùy thuộc vào quan điểm. Nó thực sự là một tình huống cực kỳ phức tạp bên dưới nắp ca-pô của một engine JS.

Vậy những chi tiết vụn vặt này tóm lại là gì? Hãy lùi lại và xem xét toàn bộ luồng của một chương trình nguồn JS:

1. Sau khi một chương trình rời khỏi trình soạn thảo của nhà phát triển, nó được transpiled bởi Babel, sau đó được đóng gói bởi Webpack (và có lẽ là nửa tá quy trình build khác), sau đó nó được chuyển đến engine JS dưới dạng rất khác đó.

2. Engine JS phân tích cú pháp mã thành AST.

3. Sau đó, engine chuyển đổi AST đó thành một loại mã byte, một biểu diễn trung gian nhị phân (IR), sau đó được tinh chỉnh/chuyển đổi thêm bởi trình biên dịch tối ưu hóa JIT.

4. Cuối cùng, JS VM thực thi chương trình.

Để hình dung lại các bước đó:

<figure>
    <img src="images/fig3.png" width="650" alt="Các bước biên dịch và thực thi JS" align="center">
    <figcaption><em>Hình 3: Phân tích cú pháp, Biên dịch và Thực thi JS</em></figcaption>
    <br><br>
</figure>

Liệu JS được xử lý giống như một script từng dòng, được thông dịch, như trong Hình 1, hay nó được xử lý giống như một ngôn ngữ biên dịch được xử lý trong một đến vài lượt đầu tiên, trước khi thực thi (như trong Hình 2 và 3)?

Tôi nghĩ rõ ràng là về mặt tinh thần, nếu không phải trong thực tế, **JS là một ngôn ngữ biên dịch**.

Và một lần nữa, lý do quan trọng là, vì JS được biên dịch, chúng ta được thông báo về các lỗi tĩnh (chẳng hạn như cú pháp sai) trước khi mã của chúng ta được thực thi. Đó là một mô hình tương tác khác biệt đáng kể so với những gì chúng ta nhận được với các chương trình "scripting" truyền thống, và được cho là hữu ích hơn!

### Web Assembly (WASM)

Một mối quan tâm chi phối đã thúc đẩy một lượng đáng kể sự phát triển của JS là hiệu suất, cả tốc độ JS có thể được phân tích cú pháp/biên dịch và tốc độ mã biên dịch đó có thể được thực thi.

Vào năm 2013, các kỹ sư từ Mozilla Firefox đã trình diễn một bản port của engine game Unreal 3 từ C sang JS. Khả năng mã này chạy trong engine JS trình duyệt ở hiệu suất 60 khung hình/giây đầy đủ được dự đoán dựa trên một tập hợp các tối ưu hóa mà engine JS có thể thực hiện cụ thể vì phiên bản JS của mã engine Unreal sử dụng một kiểu mã ưa thích một tập hợp con của ngôn ngữ JS, có tên là "ASM.js".

Tập hợp con này là JS hợp lệ được viết theo những cách hơi lạ trong mã hóa thông thường, nhưng báo hiệu một số thông tin kiểu quan trọng nhất định cho engine cho phép nó thực hiện các tối ưu hóa chính. ASM.js được giới thiệu như một cách để giải quyết áp lực về hiệu suất runtime của JS.

Nhưng điều quan trọng cần lưu ý là ASM.js chưa bao giờ được dự định là mã do các nhà phát triển biên soạn, mà là một biểu diễn của một chương trình đã được transpiled từ một ngôn ngữ khác (như C), trong đó các "chú thích" kiểu này được chèn tự động bởi công cụ.

Vài năm sau khi ASM.js chứng minh tính hợp lệ của các phiên bản chương trình do công cụ tạo ra có thể được xử lý hiệu quả hơn bởi engine JS, một nhóm kỹ sư khác (ban đầu cũng từ Mozilla) đã phát hành Web Assembly (WASM).

WASM tương tự như ASM.js ở chỗ mục đích ban đầu của nó là cung cấp một đường dẫn cho các chương trình không phải JS (C, v.v.) được chuyển đổi sang một dạng có thể chạy trong engine JS. Không giống như ASM.js, WASM đã chọn giải quyết thêm một số độ trễ vốn có trong quá trình phân tích cú pháp/biên dịch JS trước khi một chương trình có thể thực thi, bằng cách biểu diễn chương trình ở một dạng hoàn toàn không giống JS.

WASM là một định dạng biểu diễn giống với Assembly hơn (do đó có tên như vậy) có thể được xử lý bởi engine JS bằng cách bỏ qua quá trình phân tích cú pháp/biên dịch mà engine JS thường làm. Việc phân tích cú pháp/biên dịch một chương trình nhắm mục tiêu WASM diễn ra trước thời hạn (AOT); những gì được phân phối là một chương trình được đóng gói nhị phân sẵn sàng để engine JS thực thi với quá trình xử lý rất tối thiểu.

Một động lực ban đầu cho WASM rõ ràng là những cải tiến hiệu suất tiềm năng. Mặc dù đó vẫn tiếp tục là trọng tâm, WASM còn được thúc đẩy bởi mong muốn mang lại sự ngang bằng hơn cho các ngôn ngữ không phải JS đối với nền tảng web. Ví dụ, nếu một ngôn ngữ như Go hỗ trợ lập trình luồng, nhưng JS (ngôn ngữ) thì không, WASM cung cấp tiềm năng cho một chương trình Go như vậy được chuyển đổi sang một dạng mà engine JS có thể hiểu được, mà không cần tính năng luồng trong chính ngôn ngữ JS.

Nói cách khác, WASM làm giảm áp lực phải thêm các tính năng vào JS chủ yếu/dành riêng cho các chương trình được transpiled từ các ngôn ngữ khác sử dụng. Điều đó có nghĩa là sự phát triển tính năng JS có thể được đánh giá (bởi TC39) mà không bị lệch lạc bởi lợi ích/nhu cầu trong các hệ sinh thái ngôn ngữ khác, trong khi vẫn cho phép các ngôn ngữ đó có một con đường khả thi vào web.

Một quan điểm khác về WASM đang nổi lên, thật thú vị, thậm chí không liên quan trực tiếp đến web (W). WASM đang phát triển để trở thành một máy ảo (VM) đa nền tảng, nơi các chương trình có thể được biên dịch một lần và chạy trong nhiều môi trường hệ thống khác nhau.

Vì vậy, WASM không chỉ dành cho web và WASM cũng không phải là JS. Trớ trêu thay, mặc dù WASM chạy trong engine JS, ngôn ngữ JS là một trong những ngôn ngữ ít phù hợp nhất để tạo nguồn cho các chương trình WASM, bởi vì WASM dựa nhiều vào thông tin kiểu tĩnh. Ngay cả TypeScript (TS)—bề ngoài là JS + các kiểu tĩnh—cũng không hoàn toàn phù hợp (như hiện tại) để transpile sang WASM, mặc dù các biến thể ngôn ngữ như AssemblyScript đang cố gắng thu hẹp khoảng cách giữa JS/TS và WASM.

Cuốn sách này không nói về WASM, vì vậy tôi sẽ không dành nhiều thời gian thảo luận về nó, ngoại trừ việc đưa ra một điểm cuối cùng. *Một số* người đã gợi ý rằng WASM chỉ ra một tương lai nơi JS bị loại bỏ khỏi, hoặc giảm thiểu trong, web. Những người này thường nuôi dưỡng những cảm xúc tồi tệ về JS và muốn một ngôn ngữ khác—bất kỳ ngôn ngữ nào khác!—thay thế nó. Vì WASM cho phép các ngôn ngữ khác chạy trong engine JS, về mặt hình thức, đây không phải là một câu chuyện cổ tích hoàn toàn viển vông.

Nhưng hãy để tôi nói đơn giản: WASM sẽ không thay thế JS. WASM tăng cường đáng kể những gì web (bao gồm cả JS) có thể thực hiện. Đó là một điều tuyệt vời, hoàn toàn trực giao với việc liệu một số người có sử dụng nó như một lối thoát khỏi việc phải viết JS hay không.

## Nói một cách *Nghiêm túc*

Quay trở lại năm 2009 với việc phát hành ES5, JS đã thêm *chế độ nghiêm ngặt* (strict mode) như một cơ chế chọn tham gia để khuyến khích các chương trình JS tốt hơn.

Lợi ích của chế độ nghiêm ngặt vượt xa chi phí, nhưng thói quen cũ khó bỏ và quán tính của các cơ sở mã hiện có (hay còn gọi là "kế thừa") thực sự khó thay đổi. Vì vậy, thật đáng buồn, hơn 10 năm sau, *tính tùy chọn* của chế độ nghiêm ngặt có nghĩa là nó vẫn chưa nhất thiết phải là mặc định cho các lập trình viên JS.

Tại sao lại là chế độ nghiêm ngặt? Chế độ nghiêm ngặt không nên được coi là một hạn chế về những gì bạn không thể làm, mà là một hướng dẫn về cách tốt nhất để làm mọi việc để engine JS có cơ hội tốt nhất để tối ưu hóa và chạy mã hiệu quả. Hầu hết mã JS được thực hiện bởi các nhóm nhà phát triển, vì vậy *tính nghiêm ngặt* của chế độ nghiêm ngặt (cùng với các công cụ như linter!) thường giúp cộng tác trên mã bằng cách tránh một số sai lầm có vấn đề hơn trượt qua trong chế độ không nghiêm ngặt.

Hầu hết các kiểm soát chế độ nghiêm ngặt đều ở dạng *lỗi sớm*, nghĩa là các lỗi không hoàn toàn là lỗi cú pháp nhưng vẫn bị ném ra tại thời điểm biên dịch (trước khi mã được chạy). Ví dụ, chế độ nghiêm ngặt không cho phép đặt tên hai tham số hàm giống nhau và dẫn đến lỗi sớm. Một số kiểm soát chế độ nghiêm ngặt khác chỉ có thể quan sát được khi runtime, chẳng hạn như cách `this` mặc định là `undefined` thay vì đối tượng toàn cục.

Thay vì chiến đấu và tranh luận với chế độ nghiêm ngặt, giống như một đứa trẻ chỉ muốn thách thức bất cứ điều gì cha mẹ chúng bảo chúng không được làm, tư duy tốt nhất là chế độ nghiêm ngặt giống như một linter nhắc nhở bạn cách JS *nên* được viết để có chất lượng cao nhất và cơ hội tốt nhất về hiệu suất. Nếu bạn thấy mình cảm thấy bị còng tay, cố gắng làm việc xung quanh chế độ nghiêm ngặt, đó sẽ là một lá cờ cảnh báo đỏ rực rằng bạn cần phải lùi lại và suy nghĩ lại toàn bộ cách tiếp cận.

Chế độ nghiêm ngặt được bật trên mỗi tệp với một pragma đặc biệt (không có gì được phép trước nó ngoại trừ nhận xét/khoảng trắng):

```js
// chỉ khoảng trắng và nhận xét được phép
// trước pragma use-strict
"use strict";
// phần còn lại của tệp chạy ở chế độ nghiêm ngặt
```

| CẢNH BÁO: |
| :--- |
| Một điều cần lưu ý là ngay cả một dấu `;` đi lạc nằm một mình xuất hiện trước pragma chế độ nghiêm ngặt sẽ khiến pragma trở nên vô dụng; không có lỗi nào bị ném ra vì việc có một biểu thức literal chuỗi ở vị trí câu lệnh là JS hợp lệ, nhưng nó cũng sẽ âm thầm *không* bật chế độ nghiêm ngặt! |

Chế độ nghiêm ngặt có thể thay thế được bật trên phạm vi mỗi hàm, với các quy tắc chính xác tương tự về môi trường xung quanh nó:

```js
function someOperations() {
    // khoảng trắng và nhận xét đều ổn ở đây
    "use strict";

    // tất cả mã này sẽ chạy ở chế độ nghiêm ngặt
}
```

Thật thú vị, nếu một tệp đã bật chế độ nghiêm ngặt, các pragma chế độ nghiêm ngặt cấp hàm sẽ không được phép. Vì vậy, bạn phải chọn cái này hoặc cái kia.

Lý do hợp lệ **duy nhất** để sử dụng cách tiếp cận mỗi hàm đối với chế độ nghiêm ngặt là khi bạn đang chuyển đổi một tệp chương trình chế độ không nghiêm ngặt hiện có và cần thực hiện các thay đổi từng chút một theo thời gian. Nếu không, tốt hơn hết là chỉ cần bật chế độ nghiêm ngặt cho toàn bộ tệp/chương trình.

Nhiều người đã tự hỏi liệu có bao giờ JS biến chế độ nghiêm ngặt thành mặc định không? Câu trả lời là, gần như chắc chắn là không. Như chúng ta đã thảo luận trước đó xung quanh khả năng tương thích ngược, nếu bản cập nhật engine JS bắt đầu giả định mã là chế độ nghiêm ngặt ngay cả khi nó không được đánh dấu như vậy, có thể mã này sẽ bị hỏng do các kiểm soát của chế độ nghiêm ngặt.

Tuy nhiên, có một vài yếu tố làm giảm tác động trong tương lai của "sự tối nghĩa" không mặc định này của chế độ nghiêm ngặt.

Thứ nhất, hầu như tất cả mã được transpiled đều kết thúc ở chế độ nghiêm ngặt ngay cả khi mã nguồn gốc không được viết như vậy. Hầu hết mã JS trong sản xuất đã được transpiled, vì vậy điều đó có nghĩa là hầu hết JS đã tuân thủ chế độ nghiêm ngặt. Có thể hoàn tác giả định đó, nhưng bạn thực sự phải nỗ lực để làm như vậy, vì vậy rất khó xảy ra.

Hơn nữa, một sự thay đổi rộng rãi đang diễn ra theo hướng nhiều/hầu hết mã JS mới được viết bằng định dạng mô-đun ES6. Các mô-đun ES6 giả định chế độ nghiêm ngặt, vì vậy tất cả mã trong các tệp như vậy sẽ tự động được mặc định là chế độ nghiêm ngặt.

Kết hợp lại với nhau, chế độ nghiêm ngặt phần lớn là mặc định thực tế mặc dù về mặt kỹ thuật nó thực sự không phải là mặc định.

## Định nghĩa

JS là một triển khai của tiêu chuẩn ECMAScript (phiên bản ES2019 tính đến thời điểm viết bài này), được hướng dẫn bởi ủy ban TC39 và được lưu trữ bởi ECMA. Nó chạy trong các trình duyệt và các môi trường JS khác như Node.js.

JS là một ngôn ngữ đa mô hình, nghĩa là cú pháp và khả năng cho phép nhà phát triển trộn và kết hợp (và uốn cong và định hình lại!) các khái niệm từ các mô hình chính khác nhau, chẳng hạn như thủ tục, hướng đối tượng (OO/lớp) và hàm (FP).

JS là một ngôn ngữ biên dịch, nghĩa là các công cụ (bao gồm cả engine JS) xử lý và xác minh một chương trình (báo cáo bất kỳ lỗi nào!) trước khi nó thực thi.

Với ngôn ngữ của chúng ta hiện đã được *định nghĩa*, hãy bắt đầu tìm hiểu những điều cơ bản của nó.

[^specApB]: Đặc tả ngôn ngữ ECMAScript 2019, Phụ lục B: Các tính năng ECMAScript bổ sung cho trình duyệt Web, https://www.ecma-international.org/ecma-262/10.0/#sec-additional-ecmascript-features-for-web-browsers (mới nhất tính đến thời điểm viết bài này vào tháng 1 năm 2020)
