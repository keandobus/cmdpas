To pass the authentication to a called script/app, simply use the code below in your script as a called subroutine or
logic to trigger as needed. 

This example below will eat the tokens from the drive into RAM for authentication purposes. The variables
follow each called script until the CMD session has ended with an exit.

Below is a sample of how it could be done. As always, do it the way you want!


-- START CODE --

:runauth

set datad=%~dp0
set log=%datad%\log.txt
set path4app=%~dp0

call %path4app%cmdpas.bat
	if ERRORLEVEL==1 (
		echo.
		echo Unauthorized.
		pause
		exit
		)
	
rem Eat the tokens and place in RAM
			
	set /p token1=<"%datad%\acl.tok"
	del "%datad%\acl.tok"
	set /p token2=<"%datad%\session.tok"
	del "%datad%\session.tok"
	set /p token3=<"%datad%\user.tok"
	del "%datad%\user.tok"
	
	echo [%DATE% %TIME%] %token3% has logged in successfully via direct login. Session ID: %token2%>> %log%
	
goto :EOF

-- END CODE --


token1 - Numerical access level, defined in cmdpas_users_and_access.bat - default is 0-4 with 4 being the highest level.

token2 - Session key with random numbers and set markers. Use for a unique session ID.

token3 - the username of the person who logged in.