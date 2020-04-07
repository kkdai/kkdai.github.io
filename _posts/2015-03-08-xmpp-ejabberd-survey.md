---
layout: post
layout: post
title: "[XMPP][Ejabberd] Some XMPP spec survey"
description: ""
category: 
- xmpp
tags: []

---

![image](https://www.calyxinstitute.org/sites/all/images/xmpp-logo.png)



## Preface:

This article is a note about I study [XMPP](http://xmpp.org/) spec recent days. To maniplate a XMPP client, it might be easy to deal it with [IP*WORKs](http://cdn.nsoftware.com/help/IP9/bcb/XMPP.htm) 3rd party XMPP client. But it could not fulfil some custom request such as: 

-  Not response friend request immediatelly, once we got it.
-  Handle roster (friend list) programmatically
-  Maniplate VCard as custom information storage.

In those case, we might need to handle XMPP XML commands to 


## Ejabberd

![image](http://www.pihomeserver.fr/wp-content/uploads/2013/10/ejabberd_logo.jpg)

[Ejabberd](https://www.ejabberd.im/) is a [XMPP](http://xmpp.org/) server(which twitter use it at first). It written by [Erlang](http://www.erlang.org/), it is powerful to handle multiple user connectivity. 


## XMPP Command

Both we could have two ways to manipulate XMPP, one by 3rd party XMPP framework. We will use IP*Work for a example which you can find sample code from web [here](http://cdn.nsoftware.com/help/IP9/bcb/XMPP_e_SubscriptionRequest.htm).

Another one is using basic XML send to XMPP server directly. Actually we still use [IP*Works](http://cdn.nsoftware.com/help/IP9/bcb/XMPP_e_SubscriptionRequest.htm). sendCommand to send our XML to XMPP server. However, use ["SendCommand"](http://cdn.nsoftware.com/help/IP9/bcb/XMPP_m_SendCommand.htm) could be more powerful and straintforward because we handle all XMPP commands directly.

 

### Subscription Management



####  Add Subscription (Friend Request)

XMPP module should provide transfer XML command directly to server to do following path:

- [XEP-0060](http://xmpp.org/extensions/xep-0060.html)
    - Get IQ event to parse subscription request. Data format refer to [RFC-3921](http://xmpp.org/rfcs/rfc3921.html#sub)Send XML command to server use:
        
            <presence to='juliet@example.com' type='subscribe'/>

You can use IQ message from server to filter what you need.


#### Handle Subscription (Friend Request)

Once you got the subscription request from server, normally you have two way to handle it.

- If you use some XMPP framework (ex: IP*Works), you can monitor [SubscriptionRequest](http://cdn.nsoftware.com/help/IP9/bcb/XMPP_e_SubscriptionRequest.htm).
    - Note: It will need your force response "accept" or "reject" once you receipt this event. If you want to handle it in lower command set. Refer second way.
- Using low-level XML command from IQ event.


        //Receive Friend request.
        <iq from='juliet@example.com/balcony' type='get' id='roster_1'>
            <query xmlns='jabber:iq:roster'/>
        </iq>

        //accept
        <presence to='romeo@example.net' type='subscribed'/>
        //reject
        <presence to='romeo@example.net' type='unsubscribed'/>

Normally, you will need send subscription back to make sure the status subscription. (a.k.a friendship) is bi-direction.


### vCard Management

According to [XEP:00544](http://xmpp.org/extensions/xep-0054.html), vCard is an existing and widely-used standard for personal user information storage, somewhat like an electronic business card. 

It using the XML format which can store any information you need it. Detail spec refer to [RFC 2426](http://tools.ietf.org/html/rfc2426) 

The vCard information might present information as follow


#### How we use vCard?

Because vCard could be any format of XML table.

        //Abstract information from vCard
        <vCard xmlns='vcard-temp'>
            <FN>Peter Saint-Andre</FN>
            <N>
              <FAMILY>Saint-Andre</FAMILY>
              <GIVEN>Peter</GIVEN>
              <MIDDLE/>
            </N>
            <NICKNAME>stpeter</NICKNAME>
            <URL>http://www.xmpp.org/xsf/people/stpeter.shtml</URL>
            <BDAY>1966-08-06</BDAY>
            <ORG>
              <ORGNAME>XMPP Standards Foundation</ORGNAME>
              <ORGUNIT/>
            </ORG>
            <TITLE>Executive Director</TITLE>
            <ROLE>Patron Saint</ROLE>
            <TEL><WORK/><VOICE/><NUMBER>303-308-3282</NUMBER></TEL>
            <ADR>
              <WORK/>
              <EXTADD>Suite 600</EXTADD>
              <STREET>1899 Wynkoop Street</STREET>
              <LOCALITY>Denver</LOCALITY>
              <REGION>CO</REGION>
              <PCODE>80202</PCODE>
              <CTRY>USA</CTRY>
            </ADR>
            <TEL><HOME/><VOICE/><NUMBER>303-555-1212</NUMBER></TEL>
            <JABBERID>stpeter@jabber.org</JABBERID>
            <DESC>
              More information about me is located on my 
              personal website: http://www.saint-andre.com/
            </DESC>
          </vCard>

As you could observe, the vCard could store address, name, phone number... etc.

So, I am wondering if we could store some custom data in vCard for our programming usage. But here is something you should note before use it.

- vCard store as combination text:
    - vCard is store as a full text in the database, that's mean you could not able search it via specific filed.
    
- vCard is public:
    - Although vCard only modify by its owner, but it could find by everyone.
    
    
#### Enable vCard management in Ejabberd

Refer to [Ejabberd extension](http://manpages.ubuntu.com/manpages/karmic/man8/ejabberdctl.8.html). It has following API to management vCard as follow:

          //Get vCard
          vcard-get user host data [data2]
          
          //Set vCard
           vcard-set user host data [data2] content

[Note:]This command is old, need check latest one if you use Ejabberd 2.1 or newer version.
           

You can following this [instructions](https://www.ejabberd.im/mod_ctlextra?q=mod_ctlextra) to enable mod_admin_extra.


#### Management vCard data

Because there is no specific API in IP*Works to manipulate vCard, so we list it as XML commands.

Here is the detail:

        //Get vCard
        <iq from='stpeter@jabber.org/roundabout'
            id='v1'
            type='get'>
          <vCard xmlns='vcard-temp'/>
        </iq>

        //Update vCard
        <iq id='v2' type='set'>
          <vCard xmlns='vcard-temp'>
            <FN>Peter Saint-Andre</FN>
          </vCard>
        </iq>    


        //Get other's vCard
        <iq from='stpeter@jabber.org/roundabout'
            id='v3'
            to='jer@jabber.org'
            type='get'>
          <vCard xmlns='vcard-temp'/>
        </iq>


#### Conclusion about vCard usage

Because of the result of those studies, the vCard might be a way to store some extra data in your custom IM system. But I will not suggest you to store some information as follow:

- Information need to be searched by program.
    - vCard information can not search directly. You need parse it via XML tag.    
- Sensitive information
    - It is not suggest that you store some sensitive information such as password or paid information, because everyone can use XMPP protocol to access your vCard info.
        
### Reference Spec/RFCs

- [IP*Works](http://cdn.nsoftware.com/help/IP9/bcb/XMPP.htm)
    - A 3rd party network framework. It include all XMPP client implement for multiple languages.
- [RFC3921: XMPP: Instant Messaging and Presence](http://xmpp.org/rfcs/rfc3921.html)
    - It describe detail command how to get/set/change roster (subscription) from server using XMPP. We should check [RFC 6121](http://xmpp.org/rfcs/rfc6121.html) for new one.
- [RFC3920: XMPP Core](http://xmpp.org/rfcs/rfc3920.html)    
    - Old version of XMPP Core, new version is RFC 6120.
- [XEP:00544: vcard-temp](http://xmpp.org/extensions/xep-0054.html)    
    - Definition about vCard, also include how to manipulated it.    
- [RFC 2426: vCard MIME Directory Profile](http://tools.ietf.org/html/rfc2426)     
    - Detail spec about vCard format.
