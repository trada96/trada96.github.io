---
layout: post 
title: Giới thiệu tổng quan về Polkdot
group: blockchain
state: publish

---

Ở Series này chùng ta cùng nhau tìm hiểu vể polkadot là gì cũng như lợi ích giữa chúng.

# BLOCKCHAIN là gì?

- Là một sổ cái kỹ thuật số đuợc lưu trữ phân tán dựa trên công nghệ chuỗi khối

<div class="img-div" markdown="0">
    <img src="/images/polkadot/blockchain_example.jpeg" height="700"/>
</div>

**Các tính chất nổi bật của blockchain:**
- Tính bất biến: Dữ liệu trong blokchain gần như không thể sửa đổi được ( chỉ có thê sửa đổi bởi chính người đã tạo ra nó
nhưng phải được sự đồng thuận của các node trên mạng) và các dữ liệu đó sẽ được lưu mãi mãi.

- Tính minh bạch: Ai cũng có thể theo dõi được đường đi của dữ liệu trong blockchain từ địa chỉ này đến địa chỉ khác và
có thể thông kê toàn bọ lịch sử trên địa chỉ đó

- Tính phân tán: Cơ sở dữ liệu trên blokchain được lưu trữ trên các node phân tán trên toàn thế giới. Không có phiên
bản tập trung nào của cơ sở dữ liệu này tồn tại. nên hacker cũng khó có thể tấn công nó. Blockchain được lưu trữ trên
hàng triệu máy tính cùng lúc, dữ liệu của nó có thể truy cập bởi bất kì ai trên internet. 

**Các blockchain nổi bật**
- Bitcoin
- Etherum
- Binance Smart Chain
- Solana
- Near
- Polkadot
- ...

# Tổng quan kiến trúc của polkadot

Polkadot được thiết kế có mục đích để có nhiều chuỗi kết nối và hoạt động song song với nhau (multi chain)

<div class="img-div" markdown="0">
    <img src="/images/polkadot/polkadot_architecture.jpeg" height="700"/>
</div>

- ***RelayChain***: Chuỗi trung tâm của Polkadot. Nó chịu trách nhiệm kết nối các xác thực của nhiều chuỗi Para.


- ***ParaChain***: Có thể là các blockchain có chúc năng đặt biệt, có mã code riêng và tối ưu hóa chức năng của chúng
cho từng trường hợp cụ thể( Các ứng dụng thường là Defi, SmartContract, ...)


- ***ParaThreads***: Tương tự như các parachains nhưng với mô hình trả tiền khi sử dụng. Tiết kiệm hơn cho các blockchain
không cần kết nối liên tục lên mạng.


- ***BridgesChain***: Có thể hình dung nó là cầu nối giữa Polkadot Network và các blockchain khác, cung cấp khả năng tương
tác giữa các mạng lưới với nhau (Có thể kết nối với Etherum, Binance, Solana, ...).


## Ưu điểm nổi bật của Polkadot

- ***Khả năng mở rộng***: Tương tác trên các parachains được xử lý song song, cho phép các hệ thống có khả năng mở rộng cao.
Các giao dịch có thể được trải rộng trên các chuỗi, cho phép nhiều giao dịch được xử lý trong cùng một khoảng thời gian.


- ***Chia sẻ bảo mật***: Các parachains sẽ độc lập về mặt quản trị, song tính bảo mật thì luôn được đảm bảo toàn diện. Yếu
điểm PoW và PoS là cần có cộng đồng đủ lớn để đảm bảo tính bảo mật. Nhưng điều này là khá thách thức với các dự án nhỏ
và mới. Polkadot sẽ đứng ra như điểm liên kết, để các chuỗi nhỏ có thể vận hành an toàn ngay từ những ngày đầu. Các parachains
cắm vào polkadot có cùng mức án ninh.


- ***Subtrate***: Subtrate là một framework để phát triển tạo các blockchain tương thích với Polkadot, cung cấp mức độ trừu
tượng khác nhau tùy thuộc vào nhu cầu của nhà phát triển. Polkadot được xây dựng bằng subtrate. Nó làm giảm đáng kể 
thời gian, năng lượng, tiền bạc cần thiết để tạo ra một blockchain mới.


- ***Khả năng nâng cấp không cần Fork***


**Note: Các parachains chỉ tạo ra các candidate blocks, sau đó gửi sang validator của relaychain để kiểm tra, sau đó
mới tạo khối đẩy vào chuỗi chính. Các thông tin giao dịch được lưu trên parachains và chỉ đưa thông tin thay đổi
trạng thái giao dịch ( rất nhẹ) lên relaychain.**{: style="color: red; opacity: 1.0;" }


# Hệ sinh thái của Polkadot

<div class="img-div" markdown="0">
    <img src="/images/polkadot/hesinhthai.png" height="700"/>
</div>


# Lời kết 

Tất cả kiến thức trên được tham khảo từ các buổi seminar của nhóm VBI và chỉ phục vụ cho mục đích học tập cũng 
như xây dựng một cộng đồng blockchain ở Viet Nam.