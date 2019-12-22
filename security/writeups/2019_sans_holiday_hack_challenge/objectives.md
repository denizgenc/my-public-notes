1) Find the Turtle Doves

They're in the Student Union, at the north side of the quad.

2) Unredact Threatening Document

The document is on the north-west side of the quad. It's a PDF, the text of which you can fully view
if you copy and paste it.

3) Windows Log 

Note: This is only possible on a Windows machine, unfortunately, since there's an error if you try
to use the below PowerShell script on a different operating system (the script tries to use the
`Get-WinEvent` command, which requires some libraries that are exclusive to Windows, if I recall
correctly).

Clone the [DeepBlueCLI repository](https://github.com/sans-blue-team/DeepBlueCLI), download
`Security.evtx.zip` and unzip the log file in the same directory as DeepBlueCLI. Start PowerShell
in the DeepBlueCLI directory and run the command `.\DeepBlue.ps1 .\Security.evtx`
- Note that it might be slightly better to run pipe this into `Out-Host -paging`, to get paginated
  output. It's not the same as \*nix `less`, but it may be preferable to scrolling up through a
  command line window.

We're trying to find which user account was compromised. Look for the following line in the logs:
`Message : Multiple admin logons for one account` - there are three entries like this. Oddly the
objective menu only accepts one of these so keep trying till you find the right one.


