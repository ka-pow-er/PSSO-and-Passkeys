# PSSO-and-Passkeys
This is a "recipe" for the automated workflow I use to deploy passkeys in my environment, consisting of Jamf Pro (cloud) and Entra ID. It is assumed your Jamf instance is integrated with Entra ID to supply compliance info and that you are using the SSO extension.

The recipe needs to include names for extension attributes and smart groups, which of course, you are free to change to meet the needs of your environment.

The workflow includes a restart, which I and top Mac experts smarter than me, consider essential. Some would argue that the workflow "works" without a restart, and frankly it does seemingly all of the time, but you are taking a risk that impacts every Mac you manage. The Platform SSO configuration profile determines both SSO and compliance - two mission-critical aspects of your Entra ID Environment. Do you really want to gamble with your colleagues' workdays and risk future problems? Besides, the workflow makes use of a Swift Dialog message, explaining the need to restart and allowing the user the ability to close open sessions and documents.

What the user sees: 
     1) a Swift Dialog message, explaining the need to restart, 
     2) (after the restart) a macOS notification to (re-)register the Mac in Entra ID, and 
     3) a series of messages guiding the user through setting up a passkey and allowing Company Portal to auto-fill the passkey.

The basic workflow:
     1) remove the SSO config profile by un-scoping it, or have a Self Service option to do this
     2) lacking an SSO config profile, the Mac joins a smart group
     3) Macs in the smart group are the targets of a policy which
          a) "tags" the Mac to indicate it is in the middle of the passkey deployment workflow
          b) displays a Swift Dialog message, requiring a restart
     (the Mac restarts)
     4) a recon specific to Macs with the "tag" runs
     5) the Mac joins a different smart group
     6) Macs in the smart group are the targets of the Platform SSO config profile
     7) The config profile is deployed to the Mac
     8) The Mac displays a macOS notification to (re-)register the Mac in Entra ID
