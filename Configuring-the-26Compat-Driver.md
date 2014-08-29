Configure SSL example:

```cpp
#include "mongo/util/net/ssl_options.h"
#include "mongo/client/init.h"
 
int main() {
    sslGlobalParams.sslMode.store(SSLGlobalParams::SSLMode_requireSSL);
    
    // only really need a PEM on the server side
    mongo::sslGlobalParams.sslPEMKeyFile = "<path/to/keyfile.pem>";
 
    mongo::Status status = mongo::client::initialize();
 
    if (!status.isOK())
        ::abort();
 
    DBClientConnection c;
    c.connect("hostname.whatever.com"); // outgoing connections are SSL
}
```