# aria2: High-Speed Download Utility

## Description

aria2 is a lightweight multi-protocol & multi-source command-line download utility. It supports HTTP(S), FTP, SFTP, Metalink and BitTorrent. aria2 can be manipulated via JSON-RPC.

Designed for speed and minimal resource consumption, aria2 excels at downloading files from multiple sources simultaneously, maximizing your bandwidth utilization. It is particularly useful for downloading large files and for users with slow or unstable internet connections. aria2 strives to be a powerful and versatile tool for any downloading needs.

## Features

*   **Multi-Protocol Support:** HTTP(S), FTP, SFTP, Metalink, and BitTorrent.
*   **Multi-Source Downloads:** Download a single file from multiple servers simultaneously.
*   **Segmented Downloads:** Split files into segments and download them concurrently to increase speed.
*   **Metalink Support:** Download from multiple mirrors using Metalink XML files.
*   **BitTorrent Support:** Supports BitTorrent features such as DHT, PEX, Magnet URI, and UDP tracker.
*   **JSON-RPC Interface:** Control aria2 programmatically using JSON-RPC over HTTP or WebSocket.
*   **Minimal Resource Consumption:** Designed for low CPU and memory usage.
*   **Configurable:** Extensive options to customize performance and behavior.
*   **Resume Support:** Resumes interrupted downloads from where they left off.
*   **Dynamic Disk Caching:** Reduces disk I/O for faster downloads.
*   **IPv6 Support:** Supports IPv6 addresses.
*   **Daemon Mode:** Run aria2 as a background process.
*   **Cross-Platform:** Available on Linux, macOS, Windows, and other platforms.
*   **Checksum Verification:** Verify downloaded files using SHA-1, SHA-256, SHA-512, and MD5.

## Technologies Used

*   **C++:** Core implementation language for performance and efficiency.
*   **libcurl:** Used for HTTP(S), FTP, and SFTP downloads.
*   **OpenSSL:** Used for secure connections (HTTPS, SFTP).
*   **zlib:** Used for data compression and decompression.
*   **libtorrent:** Used for BitTorrent protocol implementation.
*   **JSON-RPC:** For remote control and automation.
*   **CMake:** Cross-platform build system.

## Installation

### Prerequisites

*   A C++ compiler (e.g., GCC, Clang, MSVC)
*   CMake (version 3.15 or higher)
*   libcurl development headers
*   OpenSSL development headers
*   zlib development headers
*   libtorrent development headers (optional, for BitTorrent support)

### Linux (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install build-essential cmake libcurl4-openssl-dev libssl-dev zlib1g-dev libtorrent-rasterbar-dev
git clone https://github.com/aria2/aria2.git
cd aria2
mkdir build
cd build
cmake ..
make
sudo make install
```

### Linux (Fedora/CentOS)

```bash
sudo dnf install gcc-c++ cmake libcurl-devel openssl-devel zlib-devel libtorrent-rasterbar-devel
git clone https://github.com/aria2/aria2.git
cd aria2
mkdir build
cd build
cmake ..
make
sudo make install
```

### macOS (using Homebrew)

```bash
brew install aria2
```

### Windows

1.  Download the source code from the GitHub repository.
2.  Install CMake and a C++ compiler (e.g., Visual Studio).
3.  Install the required dependencies (libcurl, OpenSSL, zlib, libtorrent) using a package manager like vcpkg or manually.
4.  Configure CMake to use your compiler and dependencies.
5.  Build the project using CMake.
6.  Add the aria2 executable to your system's PATH environment variable.

**Example using vcpkg (highly recommended):**
```bash
# From your aria2 directory
mkdir build
cd build
cmake .. -DCMAKE_TOOLCHAIN_FILE=[path to vcpkg]/scripts/buildsystems/vcpkg.cmake
cmake --build . --config Release
```
Replace `[path to vcpkg]` with the actual path to your vcpkg installation. The aria2 executable will be located in `build/Release`.

### Building without BitTorrent Support

If you wish to build aria2 without BitTorrent support (reducing dependencies), use the following CMake option:

```bash
cmake .. -DDISABLE_BITTORRENT=TRUE
```

## Usage

```bash
aria2c [options] [URIs...]
```

**Examples:**

*   Download a single file:

    ```bash
    aria2c http://example.com/file.zip
    ```

*   Download multiple files:

    ```bash
    aria2c http://example.com/file1.zip http://example.com/file2.zip
    ```

*   Download using a Metalink file:

    ```bash
    aria2c file.metalink
    ```

*   Download a torrent file:

    ```bash
    aria2c file.torrent
    ```

*   Download using a Magnet URI:

    ```bash
    aria2c magnet:?xt=urn:btih:YOUR_MAGNET_URI
    ```

*   Run aria2 as a daemon (background process):

    ```bash
    aria2c --daemon
    ```

See the `aria2c --help` output for a complete list of options.

## Configuration

aria2 can be configured using a configuration file. The default configuration file is `~/.aria2/aria2.conf`.  You can specify a different configuration file using the `--conf-path` option:

```bash
aria2c --conf-path=/path/to/my_aria2.conf http://example.com/file.zip
```

Example configuration file (`aria2.conf`):

```
dir=/path/to/download/directory
input-file=/path/to/urls.txt
max-concurrent-downloads=5
split=16
min-split-size=1M
continue=true
# HTTP/FTP options
connect-timeout=60
timeout=60
max-tries=5
# BitTorrent options
enable-dht=true
dht-listen-port=6881-6889
enable-peer-exchange=true
```

## JSON-RPC Interface

aria2 can be controlled via JSON-RPC.  To enable the JSON-RPC server, start aria2 with the `--enable-rpc` option:

```bash
aria2c --enable-rpc --rpc-listen-all=true --rpc-allow-origin-all
```

**Options:**

*   `--enable-rpc`: Enables the JSON-RPC server.
*   `--rpc-listen-all=true`: Allows connections from any IP address (use with caution).
*   `--rpc-allow-origin-all`: Allows requests from any origin (use with caution).
*   `--rpc-secret=<secret>`: Sets a secret token for authentication.  Clients must provide the secret when making requests.

You can use tools like `curl` or a dedicated JSON-RPC client library to interact with the aria2 server.

**Example using curl:**

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"aria2.getVersion","id":"0","params":[]}' http://localhost:6800/jsonrpc
```

**Authentication:**

If you have set a secret token, you must include it in the request:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"aria2.getVersion","id":"0","params":["token:<your_secret_token>"]}' http://localhost:6800/jsonrpc
```

Refer to the aria2 documentation for a complete list of available methods.

## Contributing

Contributions are welcome!  Please fork the repository, make your changes, and submit a pull request.  Follow the existing coding style and provide clear and concise commit messages.

## License

This project is licensed under the GNU General Public License version 2 (GPLv2). See the `LICENSE` file for details.