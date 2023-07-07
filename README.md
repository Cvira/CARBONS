# CARBONS
rdp ? >> signin ngrok > >and by few steps you will get it .....
 open github -  create new repository - make sure its not public - and dont do any chance later on -- just save it
    CLICK ON YOUR REPOSITORY > SETTING > SECRETS AND VARIABLES >> ACTIONS >>> ADD ACTIONS SECRET KEY -[GO TO NGROK WHERE U SIGNED IN , GO TO DASHBOARD > GETTING > YOUR AUTHTOKEN > COPY IT AND PASTE IT ON ACTIONS SECRET KEY .. ] MAKE SURE YOUR_SECRET_NAME SHOULD BE NGROK_AUTH_TOKEN ...}         

                 NOW WE HAVE TO ADD THE COMMANDS.....
    OPEN ACTIONS >>  set up a workflow yourself  {CLICK ON IT } > it should be "main.yml" in main and paste the code .. \commit changes\save as it is without editing , and press commit changes\ 
    PRESS ACTIONS { CLICK ON mail.yml / click on build / CLICK ON ENABLE / AND BOOM !!! YOU HAD CREATED AN RDP ........ 
    PASSWD AND USERNAME HAD ALSO BEEN ADDED IN IT ... PASSWD = P@ssw0rd! username = runneradmin ... } 

  PROCESS TO USE RDP - 
  OPEN REMOTE DEKSTOP CONNECTION -> 
      GO TO NGROK - ENDPOINTS {YOU WILL SEE THE CONNECTION WHICH HAD BEEN CREATED - CLICK ON IT - DONT COPY FULL URL WE HAVE NOTHING TO WITH    
 tcp:// - copy the remaining part of  or url ,  example  > 0.tcp.ngrok.io:00000 >>paste it on remote dekstop connection > AND CLICK CONNECT > AND  YOUR USERNAME PASSWD WILL BE GIVEN ON  ACTIONS {  everyones pass and username be like - [P@ssw0rd! / runneradmin]   


                |-----------------------------------------------------CARBONS---------------------------------------------------------|instagram.com/h2.td 

name: CI


on: [push, workflow_dispatch]


jobs:

  build:


    runs-on: windows-latest


    steps:

    - name: Download

      run: Invoke-WebRequest https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-windows-amd64.zip -OutFile ngrok.zip

    - name: Extract

      run: Expand-Archive ngrok.zip

    - name: Auth

      run: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN

      env:

        NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}

    - name: Enable TS

      run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0

    - run: Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

    - run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1

    - run: Set-LocalUser -Name "runneradmin" -Password (ConvertTo-SecureString -AsPlainText "P@ssw0rd!" -Force)

    - name: Create Tunnel

      run: .\ngrok\ngrok.exe tcp 3389
