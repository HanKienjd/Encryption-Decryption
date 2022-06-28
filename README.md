# Encryption-Decryption
An encryption-decryption Linux kernel module.

#### Commands: 

##### 1. For encdev character device -
	sudo mknod /dev/encdev c 89 1
	sudo chmod a+r+w /dev/encdev
	
##### 2. For decdev character device -
	sudo mknod /dev/decdev c 89 1
	sudo chmod a+r+w /dev/decdev
  
##### 3. make (command)
	1) to compile the driver and generate .ko files.
	2) compile the test files.
	
##### 4. To load the modules
	sudo insmod encdev.ko
	sudo insmod decdev.ko
	
##### 5. To unload the modules
	sudo rmmod encdev
	sudo rmmod decdev 

##### 6. To check if encdev or decdev is currently loaded or not
	lsmod | grep encdev
	lsmod | grep decdev
	
##### 7. To run the test files -
	1. For encdev
        ./test [input filename] [output filename]
	2. For decdev
        ./test [output filename]
	
- If the test function show sigkill, reload the module.
Proceed as : unloading, making and loading of module.

- Input and output files are ASCII files.

#### Encdev:
	1. Khi viết một device, kiểm tra nếu đây là lần đầu viết vào device ( sử dụng một biến plag để kiểm tra ) và nếu đúng thì chúng ta lấy key từ /dev/urandom
	2. Lưu một biến cho khóa
	3. Sau đó, tạo một mảng khác để lưu trữ dữ liệu được mã hóa hiện tại
	4. Đối với lần đầu ghi, XOR dữ liệu đầu vào với key và lưu nó trong một mảng dữ liệu mã hóa
	5. Bất kì khi nào có lần ghi tiếp theo vào thiết bị, hãy XOR dữ liệu đầu vào với mục được mã hóa cuối cùng và lưu mục hiện tại trong mảng dữ liệu mã hóa
	6. Hiện tại, khi đọc từ một device, đầu tiên gửi key tới user và sau đó gửi mảng dữ liệu mã hóa

#### TestEncDev:
	1. Tệp đầu vào chứa thông báo được mã hóa, được chuyển đổi dưới dạng đối số
	2. Khóa được tạo và ,mảng dữ liệu đã mã hóa được ghi vào file đầu ra, được chuyển đổi đưới dạng đối số 

#### Decdev:
	1. Khi viết thiết bị, kiểm tra nếu nếu đâu là lần đầu tiên viết thiết bị ( dùng biến plag để kiểm tra ) nếu đúng thì chúng ta lấy key từ file truyền vào testdecdev
	2. Đã lưu điều đó trong một biến cho khóa
	3. Sau đó, tạo một mảng khác để lưu trữ dữ liệu mã hóa hiện tại
	4. Đối với lần ghi đầu tiền, XOR dữ liệu đầu vào bằng khóa và lưu dữ liệu đó trong mảng mã hóa
	5. Bất cứ khi nào có lần ghi tiếp theo vào thiết bị, hãy XOR dữ liệu đầu vào với mục được mã hóa cuối cùng và lưu mục hiện tại trong mảng dữ liệu được giải mã
	6. Bây giờ, khi có một lần đọc từ thiết bị, trước tiên hãy gửi mảng dữ liệu đã giải mã cho người dùng

#### TestDecDev:
	1. File chứa đối số chứa thông báo được mã hóa bởi TestDecDev trước đó
	2. Hiện thị thông báo được mã hóa, giống như nội dung của file đầu vào
