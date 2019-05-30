# Modern apache2, vhosts, and other fun

* Status: [**proposed** | rejected | accepted | deprecated] 
* Deciders: greenday, mcmahon, mctastic, m1cr0man, ylmcc
* Date: 2019-May-05

Technical Story: [rbadmin issue #32](https://git.redbrick.dcu.ie/rb-admins/AdminIssues/issues/32)

## Context and Problem Statement
Apache vhosts are currently managed by a large text file located on metharme. This is completely independantly generated off of the
contents of LDAP and has to be manually triggered. The issue created by this is that changes made to the LDAP database by our tooling can have an affect on a users vhosts contents, but does nothing in this file to reflect this (update, delete, etc.) The issue this presents is that firstly, a non existent webtree that is referenced in this file causes a crash on restart. Secondly, it is yet an additional step added onto many common operations performed on users. Thirdly, it is very difficult to operate on any standard sort of way, making the current setup incompatible with redbrick 2.0

## Decision Drivers

* We are looking to automate everything and the current setup presents a limiting factor in achieving this
* This adds an entirely unnecessary level of complexity to admin and webmaster operations

## Considered Options

* LDAP entry
    * Event Hooks on actions
    * Dependancy injection on actions
    * Polling
* SQL Entry
    * Event Hooks on actions
    * Dependancy injection on actions
    * Polling

## Decision Outcome

Chosen option: "LDAP Entry", because while SQL is super performant, storing the information in SQL potentially adds a cyclical dependancy, like when we want to rename user databases to reflect their username change. This could be fixed by making databases by uid but that presents it's own issues. LDAP also makes sense as the tooling that triggers the changes that have caused issues is made to interface with LDAP and to stick with a unified system, a user rename, for example, should be able to perform all the requisit actions, otherwise, it'd be as handy to just manually change the LDAP entry and other requists by hand. This can be user in conjuction with the following [mass vhosts](https://httpd.apache.org/docs/2.4/vhosts/mass.html) - in particular the following section "Dynamic Virtual Hosts with mod_vhost_alias"

### Positive Consequences

* More extensible system with a singular source of truth that in and of itself can be master master replicated
* Less overhead when trying to perform handovers and training
* More reliable apache
* We could actually offer username changes without it being a affair

### Negative Consequences

* This has the potential to add more LDAP queries but to be quite honest that thing is hit so little you'd be worried it didn't have a pulse
* Moves the point of failure to LDAP but it at least has the potential to be much less likely of failure
