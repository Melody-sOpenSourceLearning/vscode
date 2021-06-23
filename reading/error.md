### Error

精华是实现了一套错误的分发，包括监听和取消监听。
```
export class ErrorHandler {
	private listeners: ErrorListenerCallback[];

	addListener(listener: ErrorListenerCallback): ErrorListenerUnbind {
		this.listeners.push(listener);

		return () => {
			this._removeListener(listener);
		};
	}
}
```

处理app.ts捕捉到的非预期error。

```
// app.ts
setUnexpectedErrorHandler(err => this.onUnexpectedError(err));
process.on('uncaughtException', err => this.onUnexpectedError(err));
process.on('unhandledRejection', (reason: unknown) => onUnexpectedError(reason));

// error.ts
this.unexpectedErrorHandler = function (e: any) {
	setTimeout(() => {
		if (e.stack) {
			throw new Error(e.message + '\n\n' + e.stack);
		}

		throw e;
	}, 0);
};
```

