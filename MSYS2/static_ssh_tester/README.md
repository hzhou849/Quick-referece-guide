# Static compiled C++ libssh2 application

## To Build application in msys2
1. use ```mingw32-make.exe``` msys2 is designed to use this compiler not ```make```
2. set env PATH
```bash
export PATH=/mingw64/bin:$PATH
```


## Step 0 - setup MSYS2 development enviroment

* this is the enviroment to be use develop and compile mingw in a vm container
* updated your ca-certificates before you download packages

```bash
pacman -S ca-certificates
```

* You use pacman to get mingw64 compiler packages
* set the ```PATH``` to the /mingw64/bin to have access to the binary tools
* do not use make, instead use ```mingw32-make```

## Step 1 -  Get the required libraries

* will need these static libs:

```bash
 libcrypto.a  libssh2.a  libssl.a  libz.a
```


* Start by getting the required libraries source code and build them as static .a binaries

### 1. libssh2

 i) go to github source and clone it or download from official website. 
 ii)  Using ```cmake``` configure the project to be build statically

 ```bash
 mkdir build
 cd build
 cmake -G "MinGW Makefiles" -DBUILD_SHARED_LIBS=OFF ..
 mingw32-make
 ```

* This will generate ```libssh2.a```

### 2. openssl-3.5.4

* build and generate ```libssl.a and libcrypto.a```

### 3. libz
* need to install perl to build
* build and generate ```libz.a```

### 2. Create the c++ source code
* simple ssh applicaiton to connect to client and run a command and read results
```cpp
// Static compiled version 



#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <libssh2.h>
#include <winsock2.h>
#include <ws2tcpip.h>
#include <chrono>
#include <thread>


int main() {

    // Initialize Winsock
    WSADATA wsaData;
    if (WSAStartup(MAKEWORD(2,2), &wsaData) !=0 ) {
        std::cerr << "WSAStartup failed" << std::endl;
        return 1;
    }

    const char* hostname = "195.168.0.9";
    const char* username = "zhouh5";
    const char* password = "Medtronic17";
    // const char* command = "ls -l";
    std::string command = "/home/zhouh5/Development/test_code/shmio_retriever/build/./test_code -s 1";

    const char* publickey = "/../id_rsa.pub";
    const char* privatekey = "/path/id_rsa";
    const char* passphrase = NULL;  // No passphrase used in key generation


    LIBSSH2_SESSION* session = nullptr;
    LIBSSH2_CHANNEL* channel = nullptr;
    
    
    std::cout << "Starting appliction" << std::endl;
    
    // Connect to the server
    SOCKET sock = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
    sockaddr_in sin = {0};
    sin.sin_family = AF_INET;
    sin.sin_port = htons(22);
    // sin.sin_addr.s_addr = inet_addr(hostname); // deprecated
    inet_pton(AF_INET, hostname, &sin.sin_addr);
    
    if (connect(sock, (sockaddr*)(&sin), sizeof(sin))  !=0 ) {
        std::cerr << "Failed to establish SSH session!" << std::endl;
        WSACleanup();
        close(sock);
        return -1;
    } else {
        std::cerr << "success~" << std::endl;
    }
    

    // Initialize libssh2
    session = libssh2_session_init();
    if (session == 0)
    {
        std::cerr << "Failed to initialize libbssh2" << std::endl;
        return -1;
    } else {
        std::cerr << "libssh2 successfully initialized" << std::endl;
    }

    if (libssh2_session_handshake(session, sock) != 0 ){
        std::cerr << "Failed to establish SSH session" << std::endl;
    }

    try {
        std::cerr << "authenticating..." << std::endl;
        // Option1: Authenticate with pasword
        if (libssh2_userauth_password(session, username, password)) {
            std::cerr << "Authentication failed!" << std::endl;
             libssh2_session_disconnect(session, "Bye");
            libssh2_session_free(session);
        } else {
            std::cerr << "Authentication succeeded!" << std::endl;
        }
    } catch(const std::exception& ex) {
        std::cerr << "Excpetion: " << ex.what() << std::endl;
    } catch (...) {
        std::cerr << "Unknown exception" << std::endl;
    }

    std::cerr << "authenticated!" << std::endl;
    // // Option2: using public/private keys
    // if (libssh2_userauth_publickey_fromfile(session, username, publickey, privatekey, passphrase)) {
    //     std::cerr << "Authentication failed!" << std::endl;
    // } else {
    //     std::cerr << "Authentication succeeded!" << std::endl;
    // }
    
    // Open the ssh session
    channel = libssh2_channel_open_session(session);
    
    // Execute the command
    libssh2_channel_exec(channel, command.c_str());

    // SLeep maybe needed to capture the command if the output takes a while
    // Turns out sleep is not needed
    // std::this_thread::sleep_for(std::chrono::milliseconds(5000));

    // Read the output repeatedly until done
        char buffer[1024];
        int loop_on =1;
        while (loop_on == 1) {
        int bytes_read = libssh2_channel_read(channel, buffer, sizeof(buffer));
        // std::cout << "\rbytes_read: " << bytes_read;
        if (bytes_read > 0) {
            std::cout << "" << std::string(buffer, bytes_read);
        } 
        else if (bytes_read == LIBSSH2_ERROR_EAGAIN) 
        {
            // No data yet, waiting
            std::cout << "Waiting.... for data: " << bytes_read;
        } 
        else if (bytes_read == 0) 
        {
            loop_on =0;
        }
        else
        {
            std::cout << "Error occurred!" << std::endl;
            break;
        }

    // Cleanup
    libssh2_channel_close(channel);
    libssh2_channel_free(channel);
    libssh2_session_disconnect(session, "Normal Shutdown");
    libssh2_session_free(session);
    closesocket(sock);
    libssh2_exit();

    return 0;

}
```


## 3. Use cmake to build the project

* Use this cmake script
* To build this cmake project in mysys2
```bash
mkdir build
cd build
cmake .. -G "MinGW Makefiles"
mingw32-make -j4
```

```bash
cmake_minimum_required(VERSION 3.10)
project(static_ssh_tester)

set(CMAKE_CXX_STANDARD 11)

# Include libssh library
include_directories(${CMAKE_SOURCE_DIR}/../libssh2/include)
link_directories(${CMAKE_SOURCE_DIR}/../libssh2/lib)

# Add executable
add_executable(static_ssh_tester main.cpp)

# link libssh2 statically
target_link_libraries(static_ssh_tester 
    ${CMAKE_SOURCE_DIR}/libssh2.a
    ${CMAKE_SOURCE_DIR}/libcrypto.a
    ${CMAKE_SOURCE_DIR}/libssl.a
    ${CMAKE_SOURCE_DIR}/libz.a
    ws2_32
    crypt32
)

# The shared libgcc_s_seh-1.dll, libstdc++.dll and libwinpthread-1.dll
# are part of the GCC runtime, you can configure MinGw to statically link these libraries

# libssh2: The SSH library.
# libcrypto and libssl: OpenSSL libraries.
# libz: zlib for compression.
# ws2_32: Windows Sockets library for networking.
# crypt32: Windows Cryptography API library (required by OpenSSL on Windows).
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -static-libgcc -static-libstdc++ -static")
```
