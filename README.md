# winston-syslog

This is a transport for [winston](https://github.com/flatiron/winston) that logs to
the syslog. It uses native [POSIX bindings](https://github.com/melor/node-posix) to
write to the syslog; as such, it won't work on Windows, and OS X support hasn't really
been tested. It definitely works on Linux.

It's not on NPM, yet. This was originally part of another library but has since been
factored out into its own module.

## Usage

```javascript
var winston = require('winston'),
	SyslogTransport = require('./winston-syslog');

var logger = new winston.Logger({
	transports: [
		new SyslogTransport({
			id: 'my sweet app',
			facility: 'local0',
			showPid: true
		})
	]
});
```

## Caveats
By hardcoded default, syslog does not allow messages over 1024 bytes. This transport
will cut up messages longer than that into multiple messages.

Also, `rsyslog` has a built-in rate limit of 200 messages over a period of 5 seconds
from the same process. If you're doing more logging than that, you'll need to adjust
those levels. More details [here](http://www.rsyslog.com/how-to-use-rate-limiting-in-rsyslog/).

