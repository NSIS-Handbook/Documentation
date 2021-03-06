# Dialer

The Dialer plugin for NSIS provides five functions related to internet connections.

To download files from the internet, use the NSISdl plugin.

## Usage

Simple example:

    ClearErrors           ;Clear the error flag
    Dialer::FunctionName  ;Call Dialer function
    IfErrors "" +3        ;Check for errors
      MessageBox MB_OK "Function not available"
      Quit
    Pop $R0               ;Get the return value from the stack
    MessageBox MB_OK $R0  ;Display the return value

Example function:

    ; ConnectInternet (uses Dialer plugin)
    ; Written by Joost Verburg 
    ;
    ; This function attempts to make a connection to the internet if there is no
    ; connection available. If you are not sure that a system using the installer
    ; has an active internet connection, call this function before downloading
    ; files with NSISdl.
    ; 
    ; The function requires Internet Explorer 3, but asks to connect manually if
    ; IE3 is not installed.

    Function ConnectInternet

      Push $R0

        ClearErrors
        Dialer::AttemptConnect
        IfErrors noie3

        Pop $R0
        StrCmp $R0 "online" connected
          MessageBox MB_OK|MB_ICONSTOP "Cannot connect to the internet."
          Quit ;Remove to make error not fatal

        noie3:

        ; IE3 not installed
        MessageBox MB_OK|MB_ICONINFORMATION "Please connect to the internet now."

        connected:

      Pop $R0

    FunctionEnd

### Functions

If a function is not available on the system, the error flag will be set.

#### AttemptConnect

Attempts to make a connection to the Internet if the system is not connected.

`online` - already connected / connection successful  
`offline` - connection failed  

Requires Internet Explorer 3 or later

#### AutodialOnline

Causes the modem to automatically dial the default Internet connection if the system
is not connected to the internet. If the system is not set up to automatically
connect, it will prompt the user.

Return values:

`online` - already connected / connection successful  
`offline` - connection failed  

Requires Internet Explorer 4 or later

#### AutodialUnattended

Causes the modem to automatically dial the default Internet connection if the system
is not connected to the internet. The user will not be prompted.

Return values:

`online` - already connected / connection successful  
`offline` - connection failed  

Requires Internet Explorer 4 or later

#### AutodialHangup

Disconnects an automatic dial-up connection.

Return values:

`success` - disconnection successful  
`failure` - disconnection failed  

Requires Internet Explorer 4 or later

#### GetConnectedState

Checks whether the system is connected to the internet.

Return values:

`online` - system is online  
`offline` - system is offline  

Requires Internet Explorer 4 or later

## Credits

Written by [Amir Szekely][2]. Readme by [Joost Verburg][2]

[1]: http://nsis.sourceforge.net/User:Kichik
[2]: http://nsis.sourceforge.net/User:Joost
