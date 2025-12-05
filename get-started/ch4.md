# You Don't Know JS Yet: Bắt đầu - Ấn bản thứ 2
# Chương 4: Bức tranh Lớn hơn

Cuốn sách này khảo sát những gì bạn cần biết khi bạn *bắt đầu* với JS. Mục tiêu là lấp đầy những khoảng trống mà những độc giả mới làm quen với JS có thể đã vấp phải trong những lần tiếp xúc đầu tiên với ngôn ngữ. Tôi cũng hy vọng rằng chúng tôi đã gợi ý đủ chi tiết sâu hơn trong suốt cuốn sách để khơi dậy sự tò mò của bạn muốn tìm hiểu thêm về ngôn ngữ.

Phần còn lại của các cuốn sách trong bộ sách này là nơi chúng tôi sẽ giải nén tất cả phần còn lại của ngôn ngữ, chi tiết hơn nhiều so với những gì chúng tôi có thể làm trong một vài chương ngắn ở đây.

Tuy nhiên, hãy nhớ dành thời gian của bạn. Thay vì vội vã chuyển sang cuốn sách tiếp theo trong nỗ lực nghiền ngẫm tất cả các cuốn sách một cách nhanh chóng, hãy dành chút thời gian xem lại tài liệu trong cuốn sách này. Dành thêm thời gian xem qua mã trong các dự án hiện tại của bạn và so sánh những gì bạn thấy với những gì đã được thảo luận cho đến nay.

Khi bạn đã sẵn sàng, chương cuối cùng này chia tổ chức của ngôn ngữ JS thành ba trụ cột chính, sau đó đưa ra một lộ trình ngắn gọn về những gì có thể mong đợi từ phần còn lại của bộ sách và cách tôi đề nghị bạn tiến hành. Ngoài ra, đừng bỏ qua các phụ lục, đặc biệt là Phụ lục B, "Thực hành, Thực hành, Thực hành!".

## Trụ cột 1: Phạm vi và Closure

Việc tổ chức các biến thành các đơn vị phạm vi (hàm, khối) là một trong những đặc điểm nền tảng nhất của bất kỳ ngôn ngữ nào; có lẽ không có đặc điểm nào khác có tác động lớn hơn đến cách các chương trình hoạt động.

Phạm vi giống như những chiếc xô, và các biến giống như những viên bi bạn bỏ vào những chiếc xô đó. Mô hình phạm vi của một ngôn ngữ giống như các quy tắc giúp bạn xác định viên bi màu nào đi vào xô màu tương ứng nào.

Các phạm vi lồng vào nhau, và đối với bất kỳ biểu thức hoặc câu lệnh nào, chỉ các biến ở cấp độ lồng phạm vi đó, hoặc trong các phạm vi cao hơn/bên ngoài, mới có thể truy cập được; các biến từ các phạm vi thấp hơn/bên trong bị ẩn và không thể truy cập được.

Đây là cách các phạm vi hoạt động trong hầu hết các ngôn ngữ, được gọi là phạm vi từ vựng (lexical scope). Các ranh giới đơn vị phạm vi, và cách các biến được tổ chức trong chúng, được xác định tại thời điểm chương trình được phân tích cú pháp (biên dịch). Nói cách khác, đó là một quyết định tại thời điểm tác giả: nơi bạn đặt một hàm/phạm vi trong chương trình xác định cấu trúc phạm vi của phần đó của chương trình sẽ là gì.

JS có phạm vi từ vựng, mặc dù nhiều người cho rằng không phải vậy, vì hai đặc điểm cụ thể của mô hình của nó không có trong các ngôn ngữ có phạm vi từ vựng khác.

Đầu tiên thường được gọi là *hoisting*: khi tất cả các biến được khai báo ở bất kỳ đâu trong một phạm vi được coi như thể chúng được khai báo ở đầu phạm vi. Thứ hai là các biến được khai báo bằng `var` có phạm vi hàm, ngay cả khi chúng xuất hiện bên trong một khối.

Cả hoisting và `var` phạm vi hàm đều không đủ để chứng minh cho tuyên bố rằng JS không có phạm vi từ vựng. Các khai báo `let`/`const` có một hành vi lỗi đặc biệt gọi là "Vùng Chết Tạm thời" (Temporal Dead Zone - TDZ) dẫn đến các biến có thể quan sát được nhưng không sử dụng được. Mặc dù TDZ có thể lạ lẫm khi gặp phải, nhưng nó *cũng* không phải là sự vô hiệu hóa của phạm vi từ vựng. Tất cả những điều này chỉ là những phần độc đáo của ngôn ngữ mà tất cả các nhà phát triển JS nên học và hiểu.

Closure là kết quả tự nhiên của phạm vi từ vựng khi ngôn ngữ có các hàm là giá trị hạng nhất, như JS. Khi một hàm tham chiếu đến các biến từ một phạm vi bên ngoài, và hàm đó được truyền đi như một giá trị và được thực thi trong các phạm vi khác, nó vẫn duy trì quyền truy cập vào các biến phạm vi ban đầu của nó; đây là closure.

Trên tất cả các lập trình, nhưng đặc biệt là trong JS, closure thúc đẩy nhiều mẫu lập trình quan trọng nhất, bao gồm cả các mô-đun. Theo tôi thấy, các mô-đun là *thuận theo tự nhiên* nhất có thể, khi nói đến tổ chức mã trong JS.

Để tìm hiểu sâu hơn về phạm vi, closures và cách các mô-đun hoạt động, hãy đọc Cuốn 2, *Phạm vi & Closures*.

## Trụ cột 2: Nguyên mẫu (Prototypes)

Trụ cột thứ hai của ngôn ngữ là hệ thống nguyên mẫu. Chúng ta đã đề cập sâu đến chủ đề này trong Chương 3 ("Nguyên mẫu"), nhưng tôi chỉ muốn đưa ra thêm một vài nhận xét về tầm quan trọng của nó.

JS là một trong số rất ít ngôn ngữ mà bạn có tùy chọn tạo các đối tượng trực tiếp và rõ ràng, mà không cần xác định cấu trúc của chúng trong một lớp trước.

Trong nhiều năm, mọi người đã triển khai mẫu thiết kế lớp trên các nguyên mẫu—cái gọi là "kế thừa nguyên mẫu" (xem Phụ lục A, "Các 'Lớp' Nguyên mẫu")—và sau đó với sự ra đời của từ khóa `class` của ES6, ngôn ngữ đã tăng gấp đôi xu hướng lập trình theo phong cách OO/lớp.

Nhưng tôi nghĩ rằng sự tập trung đó đã che khuất vẻ đẹp và sức mạnh của hệ thống nguyên mẫu: khả năng cho hai đối tượng chỉ cần kết nối với nhau và hợp tác động (trong quá trình thực thi hàm/phương thức) thông qua việc chia sẻ ngữ cảnh `this`.

Các lớp chỉ là một mẫu bạn có thể xây dựng dựa trên sức mạnh đó. Nhưng một cách tiếp cận khác, theo một hướng rất khác, là chỉ cần nắm lấy các đối tượng như là các đối tượng, quên hoàn toàn các lớp, và để các đối tượng hợp tác thông qua chuỗi nguyên mẫu. Điều này được gọi là *ủy quyền hành vi* (behavior delegation). Tôi nghĩ rằng ủy quyền mạnh mẽ hơn kế thừa lớp, như một phương tiện để tổ chức hành vi và dữ liệu trong các chương trình của chúng ta.

Nhưng kế thừa lớp nhận được gần như tất cả sự chú ý. Và phần còn lại dành cho lập trình hàm (FP), như một loại cách "chống lớp" để thiết kế các chương trình. Điều này làm tôi buồn, vì nó dập tắt mọi cơ hội khám phá ủy quyền như một sự thay thế khả thi.

Tôi khuyến khích bạn dành nhiều thời gian sâu trong Cuốn 3, *Đối tượng & Lớp*, để xem cách ủy quyền đối tượng nắm giữ tiềm năng lớn hơn nhiều so với những gì chúng ta có thể đã nhận ra. Đây không phải là một thông điệp chống `class`, nhưng nó cố ý là một thông điệp "các lớp không phải là cách duy nhất để sử dụng các đối tượng" mà tôi muốn nhiều nhà phát triển JS xem xét hơn.

Ủy quyền đối tượng, tôi sẽ tranh luận, *thuận theo tự nhiên* của JS hơn nhiều so với các lớp (thêm về *tự nhiên* một chút nữa).

## Trụ cột 3: Các loại và Ép kiểu

Trụ cột thứ ba của JS cho đến nay là phần bị bỏ qua nhiều nhất trong bản chất của JS.

Đại đa số các nhà phát triển có những quan niệm sai lầm mạnh mẽ về cách các *loại* (types) hoạt động trong các ngôn ngữ lập trình, và đặc biệt là cách chúng hoạt động trong JS. Một làn sóng quan tâm trong cộng đồng JS rộng lớn hơn đã bắt đầu chuyển sang các cách tiếp cận "gõ tĩnh" (static typing), sử dụng các công cụ nhận biết loại như TypeScript hoặc Flow.

Tôi đồng ý rằng các nhà phát triển JS nên tìm hiểu thêm về các loại, và nên tìm hiểu thêm về cách JS quản lý chuyển đổi loại. Tôi cũng đồng ý rằng các công cụ nhận biết loại có thể giúp các nhà phát triển, giả sử họ đã đạt được và sử dụng kiến thức này ngay từ đầu!

Nhưng tôi hoàn toàn không đồng ý rằng kết luận tất yếu của điều này là quyết định cơ chế loại của JS là tồi và chúng ta cần che đậy các loại của JS bằng các giải pháp bên ngoài ngôn ngữ. Chúng ta không cần phải tuân theo cách "gõ tĩnh" để trở nên thông minh và vững chắc với các loại trong các chương trình của mình. Có những lựa chọn khác, nếu bạn chỉ sẵn sàng đi *ngược lại dòng chảy* của đám đông, và *thuận theo tự nhiên* của JS (một lần nữa, sẽ có thêm về điều đó).

Có thể cho rằng, trụ cột này quan trọng hơn hai trụ cột kia, theo nghĩa là không có chương trình JS nào sẽ làm bất cứ điều gì hữu ích nếu nó không tận dụng đúng cách các loại giá trị của JS, cũng như việc chuyển đổi (ép kiểu) các giá trị giữa các loại.

Ngay cả khi bạn yêu thích TypeScript/Flow, bạn sẽ không tận dụng tối đa các công cụ hoặc cách tiếp cận mã hóa đó nếu bạn không quen thuộc sâu sắc với cách chính ngôn ngữ quản lý các loại giá trị.

Để tìm hiểu thêm về các loại và ép kiểu JS, hãy xem Cuốn 4, *Các loại & Ngữ pháp*. Nhưng xin đừng bỏ qua chủ đề này chỉ vì bạn luôn nghe nói rằng chúng ta nên sử dụng `===` và quên đi phần còn lại.

Nếu không học trụ cột này, nền tảng của bạn trong JS là lung lay và không đầy đủ nhất.

## Thuận theo Tự nhiên

Tôi có một số lời khuyên để chia sẻ về việc tiếp tục hành trình học tập của bạn với JS, và con đường của bạn qua phần còn lại của bộ sách này: hãy nhận thức về *tự nhiên* (grain) (nhớ lại các tham chiếu khác nhau đến *tự nhiên* trước đó trong chương này).

Đầu tiên, hãy xem xét *tự nhiên* (như trong gỗ) của cách hầu hết mọi người tiếp cận và sử dụng JS. Bạn có thể đã nhận thấy rằng những cuốn sách này đi ngược lại *tự nhiên* đó ở nhiều khía cạnh. Trong YDKJSY, tôi tôn trọng bạn, độc giả, đủ để giải thích tất cả các phần của JS, không chỉ một số phần phổ biến được chọn lọc. Tôi tin rằng bạn vừa có khả năng vừa xứng đáng với kiến thức đó.

Nhưng đó không phải là những gì bạn sẽ tìm thấy từ rất nhiều tài liệu khác ngoài kia. Điều đó cũng có nghĩa là bạn càng làm theo và tuân thủ hướng dẫn từ những cuốn sách này—rằng bạn suy nghĩ cẩn thận và tự phân tích xem điều gì là tốt nhất trong mã của mình—bạn sẽ càng nổi bật. Đó có thể là một điều tốt và xấu. Nếu bạn muốn thoát khỏi đám đông, bạn sẽ phải phá vỡ cách đám đông làm điều đó!

Nhưng tôi cũng đã có nhiều người nói với tôi rằng họ đã trích dẫn một số chủ đề/giải thích từ những cuốn sách này trong một cuộc phỏng vấn xin việc, và người phỏng vấn nói với ứng viên rằng họ đã sai; thực sự, mọi người đã được báo cáo là mất cơ hội việc làm do kết quả đó.

Càng nhiều càng tốt, tôi nỗ lực trong những cuốn sách này để cung cấp thông tin hoàn toàn chính xác về JS, được thông báo chung từ chính đặc tả. Nhưng tôi cũng đưa ra khá nhiều ý kiến của mình về cách bạn có thể diễn giải và sử dụng JS để mang lại lợi ích tốt nhất trong các chương trình của mình. Tôi không trình bày ý kiến như là sự thật, hoặc ngược lại. Bạn sẽ luôn biết cái nào là cái nào trong những cuốn sách này.

Sự thật về JS không thực sự để tranh luận. Hoặc là đặc tả nói điều gì đó, hoặc nó không. Nếu bạn không thích những gì đặc tả nói, hoặc việc tôi chuyển tiếp nó, hãy giải quyết vấn đề đó với TC39! Nếu bạn đang trong một cuộc phỏng vấn và họ cho rằng bạn sai về sự thật, hãy hỏi họ ngay tại đó và sau đó nếu bạn có thể tra cứu nó trong đặc tả. Nếu người phỏng vấn sẽ không xem xét lại, thì dù sao bạn cũng không nên muốn làm việc ở đó.

Nhưng nếu bạn chọn đồng ý với ý kiến của tôi, bạn phải chuẩn bị để sao lưu những lựa chọn đó với *lý do tại sao* bạn cảm thấy như vậy. Đừng chỉ vẹt lại những gì tôi nói. Sở hữu ý kiến của bạn. Bảo vệ chúng. Và nếu ai đó bạn đang hy vọng làm việc cùng không đồng ý, hãy bước đi với cái đầu vẫn ngẩng cao. Đó là một JS lớn, và có rất nhiều chỗ cho rất nhiều cách khác nhau.

Nói cách khác, đừng ngại đi ngược lại *tự nhiên*, như tôi đã làm với những cuốn sách này và tất cả các bài giảng của tôi. Không ai có thể cho bạn biết cách bạn sẽ sử dụng tốt nhất JS; đó là để bạn quyết định. Tôi chỉ đang cố gắng trao quyền cho bạn để đi đến kết luận của riêng bạn, bất kể chúng là gì.

Mặt khác, có một *tự nhiên* bạn thực sự nên chú ý và tuân theo: *tự nhiên* của cách JS hoạt động, ở cấp độ ngôn ngữ. Có những thứ hoạt động tốt và tự nhiên trong JS, với sự thực hành và cách tiếp cận đúng đắn, và có những thứ bạn thực sự không nên cố gắng làm trong ngôn ngữ.

Bạn có thể làm cho chương trình JS của mình trông giống như một chương trình Java, C#, hoặc Perl không? Còn Python hoặc Ruby, hoặc thậm chí PHP thì sao? Ở các mức độ khác nhau, chắc chắn bạn có thể. Nhưng bạn có nên không?

Không, tôi không nghĩ bạn nên. Tôi nghĩ bạn nên học và nắm lấy cách của JS, và làm cho các chương trình JS của bạn trở nên "JS" nhất có thể. Một số người sẽ nghĩ rằng điều đó có nghĩa là lập trình cẩu thả và không chính thức, nhưng tôi hoàn toàn không có ý đó. Tôi chỉ có ý rằng JS có rất nhiều mẫu và thành ngữ có thể nhận ra là "JS", và đi theo *tự nhiên* đó là con đường chung để đạt được thành công tốt nhất.

Cuối cùng, có lẽ *tự nhiên* quan trọng nhất để nhận ra là cách (các) chương trình hiện có mà bạn đang làm việc, và các nhà phát triển bạn đang làm việc cùng, làm mọi thứ. Đừng đọc những cuốn sách này và sau đó cố gắng thay đổi *tất cả tự nhiên đó* trong các dự án hiện tại của bạn qua đêm. Cách tiếp cận đó sẽ luôn thất bại.

Bạn sẽ phải thay đổi những điều này từng chút một, theo thời gian. Làm việc để xây dựng sự đồng thuận với các nhà phát triển đồng nghiệp của bạn về lý do tại sao việc xem lại và xem xét lại một cách tiếp cận là quan trọng. Nhưng hãy làm như vậy với chỉ một chủ đề nhỏ tại một thời điểm, và để các so sánh mã trước và sau thực hiện hầu hết các cuộc nói chuyện. Đưa mọi người trong nhóm lại với nhau để thảo luận, và thúc đẩy các quyết định dựa trên phân tích và bằng chứng từ mã thay vì quán tính của "các nhà phát triển cao cấp của chúng tôi luôn làm theo cách này".

Đó là lời khuyên quan trọng nhất tôi có thể truyền đạt để giúp bạn học JS. Luôn tiếp tục tìm kiếm những cách tốt hơn để sử dụng những gì JS cung cấp cho chúng ta để viết mã dễ đọc hơn. Mọi người làm việc trên mã của bạn, bao gồm cả bản thân bạn trong tương lai, sẽ cảm ơn bạn!

## Theo Thứ tự

Vì vậy, bây giờ bạn đã có một cái nhìn rộng hơn về những gì còn lại để khám phá trong JS, và thái độ đúng đắn để tiếp cận phần còn lại của hành trình của bạn.

Nhưng một trong những câu hỏi thực tế phổ biến nhất tôi nhận được tại thời điểm này là, "Tôi nên đọc các cuốn sách theo thứ tự nào?" Có một câu trả lời thẳng thắn... nhưng nó cũng phụ thuộc.

Đề xuất của tôi cho hầu hết độc giả là tiến hành qua loạt bài này theo thứ tự sau:

1. Bắt đầu với một nền tảng vững chắc của JS từ *Bắt đầu* (Cuốn 1) -- tin tốt, bạn đã gần hoàn thành cuốn sách này!

2. Trong *Phạm vi & Closures* (Cuốn 2), đào sâu vào trụ cột đầu tiên của JS: phạm vi từ vựng, cách điều đó hỗ trợ closure, và cách mẫu mô-đun tổ chức mã.

3. Trong *Đối tượng & Lớp* (Cuốn 3), tập trung vào trụ cột thứ hai của JS: cách `this` của JS hoạt động, cách các nguyên mẫu đối tượng hỗ trợ ủy quyền, và cách các nguyên mẫu cho phép cơ chế `class` cho tổ chức mã theo phong cách OO.

4. Trong *Các loại & Ngữ pháp* (Cuốn 4), giải quyết trụ cột thứ ba và cuối cùng của JS: các loại và ép kiểu, cũng như cách cú pháp và ngữ pháp của JS xác định cách chúng ta viết mã của mình.

5. Với **ba trụ cột** vững chắc tại chỗ, *Đồng bộ & Không đồng bộ* (Cuốn 5) sau đó khám phá cách chúng ta sử dụng kiểm soát luồng để mô hình hóa thay đổi trạng thái trong các chương trình của mình, cả đồng bộ (ngay lập tức) và không đồng bộ (theo thời gian).

6. Bộ sách kết thúc với *ES.Next & Beyond* (Cuốn 6), một cái nhìn về tương lai gần và trung hạn của JS, bao gồm một loạt các tính năng có khả năng đến với các chương trình JS của bạn trước khi quá lâu.

Đó là thứ tự dự định để đọc bộ sách này.

Tuy nhiên, các Cuốn 2, 3, và 4 thường có thể được đọc theo bất kỳ thứ tự nào, tùy thuộc vào chủ đề nào bạn cảm thấy tò mò nhất và thoải mái khám phá trước. Nhưng tôi không khuyên bạn bỏ qua bất kỳ cuốn nào trong ba cuốn sách này—thậm chí không phải *Các loại & Ngữ pháp*, như một số bạn sẽ bị cám dỗ để làm!—ngay cả khi bạn nghĩ rằng bạn đã nắm vững chủ đề đó.

Cuốn 5 (*Đồng bộ & Không đồng bộ*) rất quan trọng để hiểu sâu về JS, nhưng nếu bạn bắt đầu đào sâu và thấy nó quá đáng sợ, cuốn sách này có thể được hoãn lại cho đến khi bạn có kinh nghiệm hơn với ngôn ngữ. Bạn càng viết nhiều JS (và vật lộn với nó!), bạn sẽ càng đánh giá cao cuốn sách này. Vì vậy, đừng ngại quay lại với nó vào một thời điểm sau đó.

Cuốn sách cuối cùng trong bộ sách, *ES.Next & Beyond*, ở một số khía cạnh đứng một mình. Nó có thể được đọc ở cuối, như tôi đề nghị, hoặc ngay sau *Bắt đầu* nếu bạn đang tìm kiếm một lối tắt để mở rộng radar của mình về những gì JS là tất cả. Cuốn sách này cũng sẽ có nhiều khả năng nhận được cập nhật trong tương lai, vì vậy bạn có thể sẽ muốn xem lại nó thỉnh thoảng.

Dù bạn chọn tiến hành với YDKJSY như thế nào, hãy xem các phụ lục của cuốn sách này trước, đặc biệt là thực hành các đoạn mã trong Phụ lục B, "Thực hành, Thực hành, Thực hành!" Tôi đã đề cập rằng bạn nên đi thực hành chưa!? Không có cách nào tốt hơn để học mã hơn là viết nó.
