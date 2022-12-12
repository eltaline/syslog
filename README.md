Syslog server library for go, build easy your custom syslog server over UDP, TCP or Unix sockets using RFC3164, RFC6587 or RFC5424

Install
--------

```
go get github.com/eltaline/syslog
```

Example
--------

```go
package main

import (
        "fmt"

        "github.com/eltaline/syslog"
)

func main() {
        channel := make(syslog.LogPartsChannel)
        handler := syslog.NewChannelHandler(channel)

        server := syslog.NewServer()
        server.SetFormat(syslog.RFC5424)
        server.SetHandler(handler)
        server.ListenUDP("0.0.0.0:514")
        server.ListenTCP("0.0.0.0:514")

        server.Boot()

        go func(channel syslog.LogPartsChannel) {
                for logParts := range channel {
                        fmt.Println(logParts)
                }
        }(channel)

        server.Wait()
}
```
