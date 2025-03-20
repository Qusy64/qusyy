# LÊ Văn Quý - 24521492
# PWNABLE
**1. Hello pwner**
Thử thách này yêu cầu viết một chương trình in " Hello World " ra terminal

![Image](https://github.com/user-attachments/assets/db6ffe42-8709-43db-983c-51cdff164c66)

Ta có chương trình hello world 

![Image](https://github.com/user-attachments/assets/bb1e45bd-aa0a-4762-8d4d-70696480a46d)

Sau đó biên dịch thành tệp thực thi

![Image](https://github.com/user-attachments/assets/743605ac-df73-4f9d-b566-4a58e4e839de)

Rồi chạy

![Image](https://github.com/user-attachments/assets/78f8ead6-e467-4a4a-b759-7a0146332ded)

FLAG: W1{welcome_to_assembly}
**2. Guess me**

![Image](https://github.com/user-attachments/assets/c0ff5974-c640-4d0d-a50a-536d388758da)

Mở file thì ta thấy file nhị phân chall, em sẽ sử dụng ida64 để xem chương trình hoạt động thế nào...

![Image](https://github.com/user-attachments/assets/36c67b94-0087-44fe-be24-9dbea3fdbc65)

Nhìn vào đoạn code thì ta thấy chương trình sẽ cho biến v6 bằng một giá trị ngẫu nhiên trong đoạn từ 1 đến 100000000. Khi chạy chương trình, chúng ta sẽ nhập vào biến v4, nếu v4 = v6 thì ta sẽ có flag

![Image](https://github.com/user-attachments/assets/9871c6bc-eea7-4645-bffa-51fe66b62d05)

Nhưng với mỗi lần nhập hợp lệ, biến v5 sẽ giảm 1 đơn vị, đến khi v5 = 0, trò chơi sẽ kết thúc
Vậy chương trình chỉ cho phép nhập 27 lần.
Ta thấy để tìm một giá trị cho trước ta sử dụng thuật toán tìm kiếm nhị phân chia nhỏ khoảng giá trị và tìm được giá trị chính xác của v6.
 Điều này là khả thi khi:
 
 log₂(100000000) = 26.5754
  
 Đảm bảo sau 27 lần, ta sẽ tìm được giá trị chính xác của v6

![Image](https://github.com/user-attachments/assets/cb494597-d3bb-4a07-97f9-bd2c396b2877)

Ta có chương trình sau, chạy chương trình và ta có flag.

![Image](https://github.com/user-attachments/assets/173e93d7-02ad-4240-b390-1b3032214161)

FLAG: flag{BiNaRy_sEaRcH _iS_FuN_RiGhT_?} 
( Đoạn này em quên đổi form thành W1{...} nhưng may quá flag vẫn đúng:v)
**3. Sint**

![Image](https://github.com/user-attachments/assets/20634124-978b-4e4e-b9e2-5716fbb120d4)

Mở file code C ta thấy chương trình như sau 

![Image](https://github.com/user-attachments/assets/37f9a64a-aa69-4cf6-a878-99a6f0b456ac)

Bây giờ chúng ta cần gọi hàm win để lấy flag.
Nhìn vào code ta thấy hàm read(0, buf, size - 16) có một vấn đề. Nếu ta nhập Size = 0 thì phép trừ size - 16 sẽ gặp vấn đề cụ thể như sau.

![Image](https://github.com/user-attachments/assets/25242ce3-9465-4e5d-abd8-07dc156b25f4)

Ta thấy đây là chương trình được thiết kế để chạy trên hệ thống có kiến trúc 32-bit, và trong hàm read thì giá trị (size - 16) là một giá trị không dấu nên khi ta nhập size = 0 thì sẽ gây ra tràn số. Kết quả sẽ là 

0 - 16 = 2³² - 16 = 4294967280.

![Image](https://github.com/user-attachments/assets/60eee6e9-3c59-4054-9a78-0caf20df911f)

Thử nghiệm, ta thấy địa chỉ trả về của hàm return đã bị ghi đè. 
Giờ ta sẽ điều chỉnh dữ liệu nhập vào sao cho địa chỉ hàm win sẽ ghi đè lên địa chỉ trả về của hàm return, và sau khi hàm pwn thực thi xong thì hàm win sẽ được gọi.

![Image](https://github.com/user-attachments/assets/b846a8b7-63cd-41ce-a8ba-3335ace0dd8e)

Kiểm tra cơ chế bảo mật thì ta thấy PIE không bật, nên địa chỉ các hàm là cố định

![Image](https://github.com/user-attachments/assets/a9018b98-1923-4f8c-bcb7-8cea80922bec)

Ta có địa chỉ hàm win là 0x0804928b.

![Image](https://github.com/user-attachments/assets/b7573a61-c80b-47a8-80da-2478256ae635)

Ta thấy biến buf trỏ tới địa chỉ bộ nhớ 0xffffd050

![Image](https://github.com/user-attachments/assets/8039de50-550c-4eb7-9cb2-127e35e9f772)

Ta thấy địa chỉ bộ nhớ lưu trữ địa chỉ trả về là 0xffffd15c

![Image](https://github.com/user-attachments/assets/e13d30fa-45bd-4cb2-8864-fbfa772b44e1)

Ta có offset là 268 byte.
Và ta có script như sau

[Image](https://github.com/user-attachments/assets/bdf2e778-6879-4814-89a8-1e2eea3693d3)

![Image](https://github.com/user-attachments/assets/e0d3148c-72e5-4b99-b64f-611b048cefa4)

Và ta có flag
FLAG:W1{dbd4664aaa39cbe0e15ac1feeed0bb68b13e2d50698d2d47c4e5c84e32e31548}
