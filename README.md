## ⚠️ BE Exploit
![Build](https://img.shields.io/badge/Build-Passing!-green)

#### [WARNING] This exploit has been patched by BattleEye! You can still learn from this repository or even research further exploiting using similar tehniques. Thank you!

NOTE: As of April 2023 something similar has been confirmed but I will not be posting it publicaly on GitHub to avoid patching.

BattlEye exploit, it s**ts on their security, literally.

## Usage

1. Run build.bat (this will only work for VS19, or VS17)
2. Start an elevated commandline
3. Go to the x64/Release directory
4. implanter.exe dll_name window_class

In this case: `implanter.exe implant.dll R6Game`

To find the window's class name, use [WinLister](https://www.nirsoft.net/utils/winlister.html).

## Details

This exploits a flaw in the user-mode component of BattlEye that should've never even existed to begin with.

By hooking CreateFileW, and checking if the lpFileName parameter contains our file's name then manipulating it to believe that Kernel32.dll is being loaded, we pass their dll checks and land our module inside of it as if it's a legitimate module.


## Example

Every injectable library must have an export called "wnd_hk" that handles the WH, then calls the next in queue.

```cpp
extern "C" __declspec( dllexport )
LRESULT wnd_hk( int32_t code, WPARAM wparam, LPARAM lparam )
{
	static auto done_once = false;

	const auto pmsg = reinterpret_cast< MSG* >( lparam );

	if ( !done_once && pmsg->message == 0x5b0 )
	{
		UnhookWindowsHookEx( reinterpret_cast< HHOOK >( lparam ) );
		
		
		done_once = true;
	}

	return CallNextHookEx( nullptr, code, wparam, lparam );
}
```

The implant is a ready-to-inject example for R6:S that'll enable player icons once in-game. (Usually hidden in the prep-phase)

## Features

support for:

- thread creation
- seh, c++ exceptions
- raw detouring without any tricks
- doing literally anything you want

## Credits

DefCon42, Brit and haram <3
	

## Support

	None.
