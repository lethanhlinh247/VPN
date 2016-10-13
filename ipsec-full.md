#Tất cả về IPSec

#I. GIỚI THIỆU :

– Thuật ngữ IP Sec là một từ viết tắt của thuật Internet Protocol Security. Nó có quan hệ tới một số bộ giao thức (AH, ESP, FIP-140-1, và một số chuẩn khác) được phát triển bởi Internet Engineering Task Force (IETF). Mục đích chính của việc phát triển IP Sec là cung cấp một cơ cấu bảo mật ở tầng 3 (Network layer) của mô hình OSI, như hình :

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_1.jpg?w=604)



– Mọi giao tiếp trong một mạng trên cơ sở IP đều dựa trên các giao thức IP. Do đó, khi một cơ chế bảo mật cao được tích hợp với giao thức IP, toàn bộ mạng được bảo mật bởi vì các giao tiếp đều đi qua tầng 3. (Đó là lý do tai sao IP Sec được phát triển ở giao thức tầng 3 thay vì tầng 2).

– Ngoài ra,với IP Sec tất cả các ứng dụng đang chạy ở tầng ứng dụng của mô hình OSI đều độc lập trên tầng 3 khi định tuyến dữ liệu từ nguồn đến đích. Bởi vì IP Sec được tích hợp chặt chẽ với IP, nên những ứng dụng có thể dùng các dịch vụ kế thừa tính năng bảo mật mà không cần phải có sự thay đổi lớn lao nào. Cũng giống IP, IP Sec trong suốt với người dùng cuối, là người mà không cần quan tâm đến cơ chế bảo mật mở rộng liên tục đằng sau một chuổi các hoạt động

– IP SEC được sử dụng nhằm cung cấp sự bảo mật về dữ liệu trên đường truyền mạng lưới (LAN, INTERNET…). Người quản trị sẽ cấu hình IP SEC bằng cách thiết lập các luật giao tiếp giữa các máy. Các luật này được gọi là IP SEC Policy. Chúng xác định loại giao thức nào sẽ được mã hóa, loại nào sẽ được kí số (digital sign), và loại nào sẽ dung cả hai phương pháp trên.

– Sau đó, mỗi lần một packet được gửi đi, packet đó sẽ được kiểm tra, mã hóa, hoặc kí số tùy vào chính sách (Policy) của loại packet đó. Vì quá trình này được thực hiện tại tầng Network, nên nó hòan tòan trong suốt với người dùng và cả với các Application tạo ra các packet đó. Và vì một gói IP SEC (tạm gọi như vậy cho các gói dùng IP SEC để mã hóa) được chứa trong một gói chuẩn (standard packet), nên nó hòan tòan có thể được gửi đi trên mạng một cách bình thường.

– IP SEC cung cấp các khả năng bảo mật sau:

Chứng thực lẫn nhau trước và cả trong khi hai hệ thống giao tiếp với nhau.
Bảo đảm tính bí mật cho các gói (packet) : IP SEC bao gồm hai loại gói chính là Encapsulating Security Payload (ESP): mã hóa, chứng thực (kí số). Authentication Header (AH): chỉ chứng thực (kí số) mà không mã hóa.
Bảo toàn việc truy cập: Khi một gói được gửi đến, hệ thống sẽ kiểm tra các các ký số, nếu ký số không trùng, quá trình chứng thực không thành công. Hệ thống sẽ tự động xóa bỏ packet đó ngay tại tầng IP. ESP sẽ mã hóa các địa chỉ nguồn và địa chỉ đích và đặt trong phần payload của gói.
Ngăn chặn kiểu tấn công dùng lại: (Replay attack): Cả ESP và AH đều sử dụng các chuỗi số thay đổi. Giả sử có một attacker bắt được gói IP SEC, giả dạng gói đó để tìm cách thâm nhập vào hệ thống. Khi đó, chuỗi số mà attacker dùng sẽ không được hệ thống chấp nhận.


#II. CÁCH THỨC HOẠT ĐỘNG CỦA IP SEC :

– Giai đoạn 1 – đàm phán và tạo tunnel (đường ngầm dữ liệu):

Hai hệ thống khi giao tiếp với nhau, trước tiên, chúng sẽ trao đổi với nhau phương thức Authentication.
Việc đàm phát sẽ thông qua module Internet Key Exchange (IKE). IKE là sự kết hợp của hai giao thức: Internet Security Association and Key Management Protocol (ISAKMP) và Oakley Key Determination Protocol.
Nếu các phương thức khác nhau (ví dụ một bên dùng mã hóa Kerberos, một bên dùng chuỗi text) thì việc đàm phán sẽ kết thúc không thành công. Lưu ý là khi hai hệ thống sử dụng chuỗi nhưng có nội dung khác nhau thì cũng được coi là hai phương thức đàm phát khác nhau. Khi đã không thành công, việc kết nối của hai hệ thống coi như kết thúc.
Lúc này, dùng chương trình bắt gói để theo dõi, ta chỉ có thể thấy được các gói ISAKMP mà sẽ không thấy được các gói chứa dữ liệu AH, và ESP.
Khi việc đàm phán thành công, IKE sẽ tạo ra một tunnel để dùng cho việc trao đổi giữ liệu giữa hai hệ thống.
– Giai đoạn 2 – trao đổi dữ liệu: Trong giai đoạn này, hai hệ thống sẽ sử dụng “đường ngầm” đã tạo ra ở giai đoạn một để trao đổi giữ liệu. Trong giai đoạn này, Một module nữa là IP SEC Driver nằm ngay trong tầng Network sẽ chịu trách nhiệm mã hóa các dữ liệu được truyền đi.


#III. IP SEC PROTOCOLS – BỘ GIAO THỨC IP SEC:

1. Internet Key Exchange – IKE :

– Về cơ bản IKE được biết như ISAKMP/Oakley, ISAKMP là chữ viết tắc của Internet Security Association and Key Management Protocol, IKE giúp các bên giao tiếp hòa hợp các tham số bảo mật và khóa xác nhận trước khi một phiên bảo mật IP Sec được triển khai. Ngoài việc hòa hợp và thiết lập các tham số bảo mật và khóa mã hóa, IKE cũng sữa đổi những tham số khi cần thiết trong suốt phiên làm việc. IKE cũng đảm nhiệm việc xoá bỏ những SAs và các khóa sau khi một phiên giao dịch hoàn thành.

– Giúp cho các thiết bị tham gia trao đổi với nhau về thông tin an ninh, như mã hóa thế nào?, mã hóa bằng thuật toán gì?, bao lâu mã hóa 1 lần. IKE có tác dụng tự động thỏa thuận các chính sách an ninh giữa các thiết bị tham gia. Do đó IKE giúp cho IP Sec có thể áp dụng cho các hệ thống mạng mô hình lớn .

– Trong quá trình trao đổi key IKE dùng thuật toán mã hóa đối xứng (Symmetrical Encrytion) để bảo vệ việc trao đổi key giữa các thiết bị tham gia.

2. Encapsulation Security Payload – ESP :

– Có tác dụng xác thực (Authentication), mã hóa (Encrytion) và đảm bảo tính trọn vẹn dữ liệu (Securing of data ). Đây là giao thức được dùng phổ biến trong việc thiết lập IP Sec .

3. Authentication Header – AH :

– Có tác dụng xác thực, AH thì thường ít được sử dụng vì nó đã có trong giao thức ESP.

IV. IP SEC SECURITY ASSOCIATIONS : 

– Security Associations (SAs) là một khái niệm cơ bản của bộ giao thức IP Sec. SA là một kết nối luận lý theo một phương hướng duy nhất giữa hai thực thể sử dụng các dịch vụ IP Sec.

Các giao thức xác nhận, các khóa, và các thuật toán.
Phương thức và các khóa cho các thuật toán xác nhận được dùng bởi các giao thức Authentication Header (AH) hay Encapsulation Security Payload (ESP) của bộ IP Sec.
Thuật toán mã hóa và giải mã và các khóa.
Thông tin liên quan khóa, như khoảng thời gian thay đổi hay khoảng thời gian làm tươi của các khóa.
Thông tin liên quan đến chính bản thân SA bao gồm địa chỉ nguồn SA và khoảng thời gian làm tươi.
Cách dùng và kích thước của bất kỳ sự đồng bộ mã hóa dùng, nếu có.

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_2.jpg?w=604)

– IP Sec SA gồm có 3 trường :

SPI – Security Parameter Index : Đây là một trường 32 bit dùng nhận dạng giao thức bảo mật, được định nghĩa bởi trường Security protocol, trong bộ IP Sec đang dùng. SPI được mang theo như là một phần đầu của giao thức bảo mật và thường được chọn bởi hệ thống đích trong suốt quá trình thỏa thuận của SA.
Destination IP address : Đây là địa chỉ IP của nút đích. Mặc dù nó có thể là địa chỉ broadcast, unicast, hay multicast, nhưng cơ chế quản lý hiện tại của SA chỉ được định nghĩa cho hệ thống unicast.
Security protocol : Phần này mô tả giao thức bảo mật IP Sec, có thể là AH hoặc ESP.
– Chú thích :

Broadcasts có nghĩa cho tất cả hệ thống thuộc cùng một mạng hoặc mạng con. Còn multicasts gửi đến nhiều (nhưng không phải tât cả) nút của một mạng hoặc mạng con cho sẵn.
Unicast có nghĩa cho 1 nút đích đơn duy nhất.
– Bởi vì bản chất theo một chiều duy nhất của SA, cho nên 2 SA phải được định nghĩa cho hai bên thông tin đầu cuối, một cho mỗi hướng. Ngoài ra, SA có thể cung cấp các dịch vụ bảo mật cho một phiên kết nối được bảo vệ bởi AH hoặc ESP. Do vậy, nếu một phiên cần bảo vệ kép bởi cả hai AH và ESP, 2 SA phải được định nghĩa cho mỗi hướng. Việc thiết lập này của SA được gọi là SA bundle.

– Một IP Sec SA dùng 2 cơ sở dữ liệu. Security Association Database (SAD) nắm giữ thông tin liên quan đến mỗi SA. Thông tin này bao gồm thuật toán khóa, thời gian sống của SA, và chuỗi số tuần tự. Cơ sở dữ liệu thức hai của IP Sec SA, Security Policy Database (SPD), nắm giữ thông tin về các dịch vụ bảo mật kèm theo với một danh sách thứ tự chính sách các điểm vào và ra. Giống như firewall rules và packet filters, những điểm truy cập này định nghĩa lưu lượng nào được xữ lý và lưu lượng nào bị từ chối theo từng chuẩn của IP Sec.

#V. IP SEC SECURITY PROTOCOLS :

– Bộ IP Sec đưa ra 3 khả năng chính bao gồm :

Authentication and Data Integrity – Tính xác nhận và Tính nguyên vẹn dữ liệu : IP Sec cung cấp một cơ chế mạnh mẽ để xác nhận tính chất xác thực của người gửi và kiểm chứng bất kỳ sự sữa đổi không được bảo vệ trước đó của nội dung gói dữ liệu bởi người nhận. Các giao thức IP Sec đưa ra khả năng bảo vệ mạnh để chống lại các dạng tấn công giả mạo, đánh hơi và từ chối dịch vụ.
Confidentiality – Sự cẩn mật : Các giao thức IP Sec mã hóa dữ liệu bằng cách sử dụng kỹ thuật mã hóa cao cấp, giúp ngăn cản người chưa chứng thực truy cập dữ liệu trên đường đi của nó. IP Sec cũng dùng cơ chế tạo hầm để ẩn địa chỉ IP của nút nguồn (người gửi) và nút đích (người nhận) từ những kẻ nghe lén.
Key management – Quản lý khóa : IP Sec dùng một giao thức thứ ba, Internet Key Exchange (IKE), để thỏa thuận các giao thức bao mật và các thuật toán mã hóa trước và trong suốt phiên giao dịch. Một phần quan trọng nữa, IP Sec phân phối và kiểm tra các khóa mã và cập nhật những khóa đó khi được yêu cầu.
– Hai tính năng đầu tiên của bộ IP Sec, Authentication and Data Integrity, và Confidentiality, được cung cấp bởi hai giao thức chính của trong bộ giao thức IP Sec. Những giao thức này bao gồm Authentication Header (AH) và Encapsulating Security Payload (ESP).

– Tính năng thứ ba, key management, nằm trong bộ giao thức khác, được bộ IP Sec chấp nhận bởi nó là một dịch vụ quản lý khóa mạnh. Giao thức này là IKE.


#VI. CÁC CHẾ ĐỘ IP SEC :

– SAs trong IP Sec hiện tại được triển khai bằng 2 chế độ. Được mô tải ở hình dưới đó là chế độ Transport và chế độ Tunnel. Cả AH và ESP có thể làm việc với một trong hai chế độ này

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_3.jpg?w=604)


##1. Transports mode :

– Transport mode bảo vệ giao thức tầng trên và các ứng dụng. Trong Transport mode, phần IP Sec header được chèn vào giữa phần IP header và phần header của giao thức tầng trên, như hình mô tả :


![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_4.jpg?w=604)


– Transport mode thiếu mất quá trình xữ lý phần đầu, do đó nó nhanh hơn. Tuy nhiên, nó không hiệu quả trong trường hợp ESP có khả năng không xác nhận mà cũng không mã hóa phần đầu IP.

– Dữ liệu (Layer4 Payload) được mã hóa sẽ nằm trong ESP header và ESP sẽ chèn vào giữa Layer 2 header và layer 3 header

##2. Tunnel mode :

– Không giống Transport mode, Tunnel mode bảo vệ toàn bộ gói dữ liệu. Toàn bộ gói dữ liệu IP được đóng gói trong một gói dữ liệu IP khác và một IP Sec header được chèn vào giữa phần đầu nguyên bản và phần đầu mới của IP.

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_6.jpg?w=604)

– Trong AH Tunnel mode, phần đầu mới (AH) được chèn vào giữa phần header mới và phần header nguyên bản, như hình bên dưới.


![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_7.jpg?w=604)


– Dữ liệu sẽ được mã hóa và đóng gói thành 1 IP Header mới với source và des IP mới.

– IP Sec có những phương pháp mã hóa như DES (Data Encrution Standard) , 3DES , AES (Advance Encrytion Standar).

– IP Sec có những phương pháp xác thực như HMAC , MD5 , SHA-1

#VII. CƠ CHẾ HOẠT ĐỘNG CỦA BỘ GIAO THỨC IP SEC – MỞ RỘNG III :

##1. Internet Key Exchange – IKE :

– Cơ chế hoạt động của IKE :

Như đã nói ở trên, giao thức IKE sẽ có chức năng trao đổi key giữa các thiết bị tham gia VPN và trao đổi chính sách an ninh giữa các thiết bị.
Nếu không có giao thức này thì người quản trị phải cấu hình thủ công, và những chính sách an ninh trên những thiết bị này được gọi là SA (Security Associate). Do đó các thiết bị trong quá trình IKE sẽ trao đổi với nhau tất cả những SA mà nó có. Các thiết bị này sẽ tự tìm ra cho mình những SA phù hợp với đối tác nhất.
Những key được trao đổi trong quá trình IKE cũng được mã hóa và những key này sẽ thay đổi theo thời gian (Generate Key) để tránh tình trạng BruteForce của Attacker.
– Các giai đoạn hoạt động của IKE bao gồm :

###1A. IKE Phases :

– Giai đoạn I và II là hai giai đoạn tạo nên phiên làm việc dựa trên IKE, hình bên dưới trình bày một số đặc điểm chung của hai giai đoạn. Trong một phiên làm việc IKE, nó giả sử đã có một kênh bảo mật được thiết lập sẵn. Kênh bảo mật này phải được thiết lập trước khi có bất kỳ thỏa thuận nào xảy ra.

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_9.jpg?w=604)


** IKE Phases 1 – Bắt buộc xảy ra trong quá trình IKE :**

– Giai đoạn I của IKE đầu tiên xác nhận các điểm thông tin, và sau đó thiết lập một kênh bảo mật cho sự thiết lạp SA.

– Tiếp đó, các bên thông tin thỏa thuận một ISAKMP SA đồng ý lẫn nhau, bao gồm các thuật toán mã hóa, hàm băm, và các phương pháp xác nhận bảo vệ mã khóa.

– Sau khi cơ chế mã hóa và hàm băm đã được đồng ý ở trên, một khóa chi sẽ bí mật được phát sinh. Theo sau là những thông tin được dùng để phát sinh khóa bí mật :

Giá trị Diffie-Hellman
SPI của ISAKMP SA ở dạng cookies
Số ngẩu nhiên known as nonces (used for signing purposes)
Nếu hai bên đồng ý sử dụng phương pháp xác nhận dựa trên Public Key, chúng cũng cần trao đổi IDs. Sau khi trao đổi các thông tin cần thiết, cả hai bên phát sinh những key riêng của chính mình sử dụng chúng để chia sẽ bí mật. Theo cách này, những khóa mã hóa được phát sinh mà không cần thực sự trao đổi bất kỳ khóa nào thông qua mạng.

** IKE Phases 2 – Bắt buộc xảy ra trong quá trình IKE :**

– Trong khi giai đoạn I thỏa thuận thiết lập SA cho ISAKMP, giai đoạn II giải quyết bằng việc thiết lập SAs cho IP Sec. Trong giai đoạn này, SAs dùng nhiều dịch vụ khác nhau thỏa thuận. Cơ chế xác nhận, hàm băm, và thuật toán mã hóa bảo vệ gói dữ liệu IP Sec tiếp theo (sử dụng AH và ESP) dưới hình thức một phần của giai đoạn SA.

– Sự thỏa thuận của giai đoạn xảy ra thường xuyên hơn giai đoạn I. Điển hình, sự thỏa thuận có thể lặp lại sau 4-5 phút. Sự thay đổi thường xuyên các mã khóa ngăn cản các hacker bẻ gãy những khóa này và sau đó là nội dung của gói dữ liệu.
– Tổng quát, một phiên làm việc ở giai đoạn II tương đương với một phiên làm việc đơn của giai đoạn I. Tuy nhiên, nhiều sự thay đổi ở giai đoạn II cũng có thể được hổ trợ bởi một trường hợp đơn ở giai đoạn I. Điều này làm qua trình giao dịch chậm chạp của IKE tỏ ra tương đối nhanh hơn.

###1B. IKE Modes :

– Có 4 chế độ IKE phổ biến thường được triển khai :

Chế độ chính – Main mode (thuộc giai đoạn I).
Chế độ linh hoạt – Aggressive mode (thuộc giai đoạn I).
Chế độ nhanh – Quick mode (thuộc giai đoạn II).
Chế độ nhóm mới – New Group mode (thực hiện sau giai đoạn I, nhưng nó không thuộc giai đoạn II.
– Ngoài 4 chế độ IKE phổ biến trên, còn có thêm Informational mode. Chế độ này kết hợp với quá trình thay đổ của giai đoạn II và SAs. Chế độ này cung cấp cho các bên có liên quan một số thông tin thêm, xuất phát từ những thất bại trong quá trình thỏa thuận. Ví dụ, nếu việc giải mã thất bại tại người nhận hoặc chữ ký không được xác minh thành công, Informational mode được dùng để thông báo cho các bên khác biết.

** Main Mode :**

– Main mode xác nhận và bảo vệ tính đồng nhất của các bên có liên quan trong qua trình giao dịch. Trong chế độ này, 6 thông điệp được trao đổi giữa các điểm :

2 thông điệp đầu tiên dùng để thỏa thuận chính sách bảo mật cho sự thay đổi.
2 thông điệp kế tiếp phục vụ để thay đổi các khóa Diffie-Hellman và nonces. Những khóa sau này thực hiện một vai tro quan trọng trong cơ chế mã hóa.
Hai thông điệp cuối cùng của chế độ này dùng để xác nhận các bên giao dịch với sự giúp đỡ của chữ ký, các hàm băm, và tuỳ chọn với chứng nhận.

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_10.jpg?w=604)

** Aggressive Mode :**

– Aggressive mode về bản chất giống Main mode. Chỉ khác nhau thay vì main mode có 6 thông điệp thì chết độ này chỉ có 3 thông điệp được trao đổi. Do đó, Aggressive mode nhanh hơn mai mode. Các thông điệp đó bao gồm :

Thông điệp đầu tiên dùng để đưa ra chính sách bảo mật, pass data cho khóa chính, và trao đổi nonces cho việc ký và xác minh tiếp theo.
Thông điệp kế tiếp hồi đáp lại cho thông tin đầu tiên. Nó xác thực người nhận và hoàn thành chính sách bảo mật bằng các khóa.
Thông điệp cuối cùng dùng để xác nhận người gửi (hoặc bộ khởi tạo của phiên làm việc).

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_11.jpg?w=604)

** Quick Mode :**

– Chế độ thứ ba của IKE, Quick mode, là chế độ trong giai đoạn II. Nó dùng để thỏa thuận SA cho các dịch vụ bảo mật IP Sec. Ngoài ra, Quick mode cũng có thể phát sinh khóa chính mới. Nếu chính sách của Perfect Forward Secrecy (PFS) được thỏa thuận trong giai đoạn I, một sự thay đổi hoàn toàn Diffie-Hellman key được khởi tạo. Mặt khác, khóa mới được phát sinh bằng các giá trị băm :

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_12.jpg?w=604)


** New Group Mode :**

– New Group mode được dùng để thỏa thuận một private group mới nhằm tạo điều kiện trao đổi Diffie-Hellman key được dễ dàng. Hình 6-18 mô tả New Group mode. Mặc dù chế độ này được thực hiện sau giai đoạn I, nhưng nó không thuộc giai đoạn II.

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_13.jpg?w=604)


##2. Authentication Header và Encapsulation Security Payload :

– Đây là hình minh họa việc đóng gói dự liệu bằng 2 protocol ESP và AH . Trên cùng là gói dữ liệu nguyên thủy bao gồm Data và IP Header .

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_14.jpg?w=604)

– Nếu sử dụng giao thức ESP : Thì giao thức ESP sẽ làm công việc là mã hóa (Encryption ), xác thực (Authentication ), bảo đảm tính trọn vẹn của dữ liệu (Securing of data). Sau khi đóng gói xong bằng ESP mọi thông tin về mã hóa và giải mã sẽ nằm trong ESP Header.Các thuật toán mã hóa bao gồm DES , 3DES , AES. Các thuật toán để xác thực bao gồm MD5 hoặc SHA-1. ESP còn cung cấp tính năng anti-relay để bảo vệ các gói tin bị ghi đè lên nó.

– Nếu sử dụng giao thức AH : Thì giao thức AH chỉ làm công việc xác thực (Authentication ) và bảo đảm tính trọn vẹn dự liệu. Giao thức AH không có chức năng mã hóa dự liệu. Do đó AH ít được dùng trong IP Sec vì nó không đãm bảo tính an ninh.

– Lưu ý quá trình xác thực và mã hóa của ESP và AH chỉ diễn ra sau khi quá trình IKE hòan thành.

###2A. Authentication Header – AH :

– Dưới đây là hình minh họa về cơ chế xác thực của giao thức AH :

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_15.jpg?w=604)

– Bước thứ 1 : Giao thức AH sẽ đem gói dữ liệu (Packet ) bao gồm Payload + IP Header + Key cho chạy qua 1 giải thuật gọi là giải thuật Hash và cho ra 1 chuỗi số. Đây là giải thuật 1 chiều, nghĩa là từ gói dữ liệu + key = chuỗi số. Nhưng từ chuỗi số không thể Hash ra = dữ liệu + key. Và chuỗi số này sẽ đuợc gán vào AH header .

– Bước thứ 2 : AH Header này sẽ được chèn vào giữa Payload và IP Header và chuyển sang phía bên kia. Đương nhiên ta cũng nhớ cho rằng việc truyền tải gói dự liệu này đang chạy trên 1 tunel mà trước đó quá trình IKE sau khi trao đổi khóa đã tạo ra .

– Bước thứ 3 : Router đích sau khi nhận được gói tin này bao gồm IP Header + AH Header + Payload sẽ được chạy qua giải thuật Hash 1 lần nữa để cho ra 1 chuỗi số .

– Bước thứ 4 : So sánh chuỗi số nó vừa tạo ra và chuỗi số của nó nếu giống nhau thì nó sẽ chấp nhận gói tin. Nếu trong quá trình truyền gói dữ liệu 1 Attacker Sniff nói tin và chỉnh sửa nó dẫn đến việc gói tin bị thay đổi về kích cỡ, nội dung thì khi đi qua quá trình Hash sẽ cho ra 1 chuỗi số khác chuỗi số ban đầu mà Router đích đang có . Do đó gói tin sẽ bị Drop.

– Thuật toán hash bao gồm MD5 và SHA-1. Trong trừong hợp này IP Sec đang chạy ở chế độ Trasports Mode .

###2B. Encapsulation Security Payload – ESP :

– Phía dưới đây là cơ chế mã hóa gói dữ liệu bằng giao thức ESP :

![](https://duongtuanan.files.wordpress.com/2012/10/102912_1413_16.jpg?w=604)

– ÉP là giao thức hỗ trợ cả xác thực và mã hóa. Phía trên là gói dữ liệu ban đầu và ESP sẽ dùng 1 cái key để mã hóa toàn bộ dữ liệu ban đầu. Trường hợp này, IP Sec đang chạy ở chế độ Tunel Mode nên ngoài ESP Header ra nó sẽ sinh ra 1 IP Header mới không bị mã hóa để có thể truyền đi trong mạng Internet.