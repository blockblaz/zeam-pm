# Zeam Weekly call - Meeting Notes and Transcript
# Date: September 19, 2025

## Meeting Overview
**Date:** September 19, 2025
**Recording:** https://youtu.be/5n_A4t9Becs

---
## Meeting Notes

Sep 19, 2025
Zeam call  - Transcript

00:00:00
 
Gajinder Singh: Hello everyone, welcome to September 19 Zoom call. We will uh go through updates and we will then take uh what is uh measure of our progress for DevNet zero and then uh the floor will be open to discuss any issues any outstanding issues that we want to discuss or anything that we want to talk about upcoming events. So this time I will go first. Uh so I have been reviewing PRs merging in things so that uh we are Devnet zero ready and uh uh thanks for some of the good PRs out there. Also uh I'm currently looking at uh Chetany's integration PR uh which uh which basically uh uh is breaking on the build and what is happening is most likely related to SSZ because uh there so while while while uh when we whenever we use SSZ root or something Uh so compile time I think the code is generated and what is happening is that uh uh when there are signed votes in it then apparently uh the hash root of the block is uh going for stack too high so or something like that.
 
 
00:01:34
 
Gajinder Singh: But I think uh what is happening could be related to bounded array as well because underlying list for example Chetany's PR is about Chetany SPR is about to transform uh uh slices to boundary list and uh what and boundary boundary list is uh to the list and our our list is sort of under uh the inner element of our list is uh backed by a bounded list and it is not using allocator. So I am sort of assuming that there is some sort of issue regarding that but uh I'm not sure why it's happening at the build time. Uh so build time basically there is a warning and an error that is coming up and I'm looking into that and while Chetany is also looking into that. Apart from that what are the things outstanding? Uh, I guess we'll talk about Devnet zero readiness post updates. passing on to Gim.
Guillaume Ballet: Hey, no big updates. Uh, just doing um just doing PR reviews like I'm currently looking at something by Chetany. Um, yeah, I hope to be able to merge it uh today like u in the next hour actually.
 
 
00:02:54
 
Guillaume Ballet: Uh, but yeah, trying to to investigate a few things. Um, and then there was this thing by by Mercy that I was also looking at, but yeah, uh, I'll I'll take care I'll take care of it afterwards. Um, I I'm a bit behind like I've been uh held back by those two PRs. I will I will try to catch up all the the open PRs we've got.
Gajinder Singh: We can now move to Kai.
Kai Chen: uh uh most most the work uh for me is about uh uh start uh start node for net zero. Yeah. So I I do some refactor and uh and right now there is an issues uh in the in how to test how to test it in uh start two nodes in one process. Uh this is the issue mercy mercy meet. So uh maybe we can we can take a look it then. Yeah. So for for me next week uh I will take a look the the issue about uh correct topic URL. Subscribe.
Gajinder Singh: So uh so Kai there are a couple of issues I think uh that you should take a look at.
 
 
00:04:35
 
Gajinder Singh: One is basically subscribing the correct topic URL which I don't think is going to be a big deal. Then the second would be to use TLS uh instead of noise in our uh rust lip bridge. uh but the main thing I think that maybe you should basically you know focus towards is integrating uh the spec test uh that Tomma mentioned about I think we are at that stage where we should uh uh basically have a very basic uh spec test integration maybe you know just couple of spec test that we pick from there and uh do it then basically once we have the framework already uh baked in then we can distribute that task amongst other team members as So I think that is the sort of a big chunk that I think uh that you can focus on next week and sort of handle it.
Kai Chen: Uh, okay. Okay. For the TIS instead noise. Uh, because I already enabled the quick transport in the rust libp2p bridge. Uh maybe I think uh there is no effort for for us to do that because I think the creek already has TLS support.
 
 
00:06:00
 
Gajinder Singh: Uh so by default does quick work on TLS or do we have to enable it somewhere?
Kai Chen: Yeah. Yeah. Uh there is a flag there is a flag in the co in the rust libp2p bridge. I already in uh I already changed that flag uh from force to true. So right now we already used the quick transport.
Gajinder Singh: but the quick transport that is already using TLS.
Kai Chen: I yes I think so.
Gajinder Singh: Uh so basically just uh double check it if you're not totally sure about it.
Kai Chen: Okay. Okay.
Gajinder Singh: All right. And then I think uh so we might some other some other might person might pick this task which would be to add the request response methods but I guess that we can you know uh hand out to someone else who could be interested in that. So right now then we can move to Mercy.
Mercy Boma: Good afternoon. Um, so I don't have much updates, but I have like a lot of pending um, discussions regards to my updates.
 
 
00:07:20
 
Mercy Boma: For example, the Genesis node um, that I'm trying to run, I have a problem with it. I did tag you and Gum to it on Discord on sorry, GitHub. Then the other one, the C light test, I think I'm waiting for your final review on it because I finished up with the um the separation you asked me to do. And there's another thing, the other test we supposed to um um the unit test for the um it two network. Um there's they kind of like Kai complained about it not being like a real test. It was all on mock but the reason was that we have a lot of pending to-dos in order for us to like actually get the real implementation. For example, um there's this to-do figure out why schedule of the loop is not working for lip. Um it's I think around interface.zig that is where the code is. And there's another thing I can't recall right now. We have a me memory management issue. That's another thing I I another thing else I saw.
 
 
00:08:37
 
Mercy Boma: So, I don't know if
Gajinder Singh: Right. So I think so I think it's not good to spread ourselves like that and focus and get something out rather than doing 10 things and not being able uh to deliver uh anything because uh uh I mean the point is that once you conclude something then basically you carry on the results of that thing to to your other PRs as well and for that uh yes so first we need to I will again review and verify uh uh your uh uh CLI PR which I think is the first thing to do. Then I will uh then once it is done then uh uh I think you are doing the uh node spin up same PR right and that is on that the P is already up all right so then I
Mercy Boma: Yes. Yes.
Gajinder Singh: will take a look at that uh and then basically we'll come to other issues uh for ETL P2B network mock network just I mean uh don't right now spend time at one of my particular comments which was basically scheduling u uh scheduling the data that you get from ETH lily P2P onto onto the loop because uh there is already an ETH lip P2P loop running which is scheduling the data.
 
 
00:10:10
 
Gajinder Singh: So basically ignoring that for now that is not uh an issue to focus on with regard to uh freeing of the data. Uh so KA's PR has been merged. Uh and yes so we we should basically take it in an incremental way. uh but one of the primary things that is needed to free data which the most of the data is SSZ uh allocated data and we basically need uh uh SSZ primitives to recursively free them any slices or uh uh or any you know list that are that are inside it. But uh looking at the list implementation it is bounded array and uh so its allocation is most probably happening on stack right Kai right so we might want to change
Kai Chen: Yes.
Gajinder Singh: its allocation to happen on uh uh on the um on the heap because uh uh the sizes can grow very big. For example, now that we have signed votes, right? the each uh the signature of vote itself is 4,000 bytes and that could lead to a heavy uh allocation on stack and again the uh we don't want it to be allocated on stack because uh once for example SSZ types are created and then they are moved around uh so we probably don't want it to be on stack and being copied here and there right so that is uh definitely one of the issues that I I
 
 
00:11:54
 
Gajinder Singh: think are coming up from Chetany SPR SSZ list PR as well. So we need to basically refactor some of the SSZ add some methods onto it and uh so we we need to look at it at a holistic way and start from there. Uh but if there is something that is getting in your way, yes do solve it right now. But if something is not getting in your way as of now, then basically put it aside. Create an issue for it. If you think you know you have spotted something that is already not there, create an issue and then somebody will pick it up and run with it. But uh important thing is to maintain the focus and to deliver what you are working on. So for you Mercy it is basically uh the CLI test and uh I will go back and review your PR uh and then it's about node CLI test. So these are the things that that are on your plate and uh then basically we can also uh then add some of uh the APIs that need that uh we need to serve for example for ENR uh so Kai did comment on that particular issue but uh I don't remember a full conversation on it where we have to basically uh so we basically add some of the beacon end points which are relevant to us for serving
 
 
00:13:21
 
Gajinder Singh: the ENR as well as for uh ser for enabling uh June's uh visualization tool. So we will do that. Uh but yes uh uh there are other tasks that are allocated as well. Uh that could be higher priority. The other the task they are in the they are added in the issues as well. So there could be higher priority and we basically will pick from that once the CLI task is done. So yeah. All right. Uh let's move to Chetany.
Chetany Bhardwaj: one. So my updates have been covered but I'll go through them more or less. So first is what I worked on was uh integrate like adding more validation and test to the SSZ uh which included we I bought a lot of test uh from the current beacon uh like validators to our SSZ libraries. So over that side all the test uh right now are passing. We did uh there were some changes to uh state root calculation and uh very minor changes in uh how we uh there there were some handling cases for especially for booleans I can remember that were not there added those uh there are also suggestions for adding a few more things so I'm looking into that and also made a draft PR for integrating this uh SSZ into uh the Zam codebase since now we have the list bit
 
 
00:15:07
 
Chetany Bhardwaj: list types and uh decent testing around that and as mentioned yes so the the test prime like uh the issues primarily stem probably from boundary error that is what I learned as well and given the current uh values for that we have foret zero the justification uh ports are coming somewhere around 134 4 megabytes right now which is uh not something bounded array is designed for. So we my u I'm again looking into this and uh the the exact I also u modified the PR if anybody wants to uh reproduce the errors. So the build will be successful. You can just uh do a build test for it. And there is a specific index error as well. Uh which I I I did some digging into. It seems like it's a not something being calculated but it is a generic uh specifier value that the compiler uses. Uh the the one that I shared with you today, Gajinder. So it's not from our codebase. It's when something has been freed from memory and we try to access it.
 
 
00:16:18
 
Chetany Bhardwaj: It's just a a fix specific value that is often used uh which is 0x a aaa till the end. So I'm just looking into this today and trying to come up with uh a alternative approach to fix this because again bounded arrays are not built for somewhere uh for handling sizes the that we have right now which might even grow in the future. So that is that something that I'm working on right now.
Gajinder Singh: All right. So maybe let's do a quick and dirty PR quick PR to replace bounded array with array list. I I don't think it's going to be that difficult. So So instead of me trying to debug boundary list, I don't want to I don't want to debug boundary list at all.
Chetany Bhardwaj: Yes, a one to one change to array list should be I mean it it sounds easier. So uh I'll I'll get that going and see if that works.
Guillaume Ballet: Uh, sorry. We're not doing No, please.
Gajinder Singh: So can you just do a quick PR to change to array list and then we'll go back.
 
 
00:17:23
 
Guillaume Ballet: Let's not do this before uh the actual dev nets. Yeah, this is going to break a tunnel. Thanks. Like, can we can we just wait for quieter time? We we just uh said we will fix 0.141 and that's good enough. Um, let's not uh Yeah, please let's not add more difficulty like But they will not work in 051. Right?
Gajinder Singh: But bounded array might not work right.
Guillaume Ballet: They they work in 151. Sorry, in 141.
Chetany Bhardwaj: No, that's still failing with us.
Gajinder Singh: No, we we are not talking about upgrading the zig compiler. we are talking about uh uh build breaking and uh basically you know I don't think that bounded array is going to support sizes that we will have for signed votes for example I don't know I haven't seen but I mean they are getting on stack and we are having stack uh build error I think when we build let me see what the error it says it says stack frame.
Guillaume Ballet: Yeah. Okay. Okay.
 
 
00:18:31
 
Guillaume Ballet: Let's review the PR because maybe
Gajinder Singh: Yeah, basically just there is some stack frame error when you're trying to build it. Most likely coming from bounded array, but I'm not sure about it. Uh but at some point I think we will need to replace the bounded array with array list. I mean it's not about the compiler version.
Guillaume Ballet: I I Okay. Uh if it's a stack frame issue, I mean, it's definitely the compiler because this should uh Okay. I don't know what you put on the stack. Maybe uh maybe we're putting too much data on the stack, but I would say that's not that's not a a problem that we that is due to the boundary, right? Okay, fine. Uh let's not hold the the thing for that. Let's let's just review and agree. But I would just say let's not replace it all over the place. Uh if it turns out uh depends what you have in your SSZ object but yeah okay uh let's let's see let's see uh let's take it
 
 
00:19:20
 
Gajinder Singh: No, but you can't you can't have your SSZ objects on stack, right? For example, block and state uh they are allocated on stack. We can't have that. Cool. Uh, all right. So, let's Yes.
Chetany Bhardwaj: It's so sorry I I uh didn't catch it in the end. So should should I work on the replacement to array list or should I hold that for now?
Gajinder Singh: I think doing a PR would not be a problem. uh if unless you have already figured out uh where the issue is. If you haven't figured out where the issue is, I think doing the PR and using that commit to check whether we are able to successfully resolve our issues. I think that would be fine even though we might not merge that PR right now.
Chetany Bhardwaj: Got it. Got it. So, so I'll just make a testing p just to see if it works.
Gajinder Singh: just put the PR in the draft status and then use that particular commit to link the SSZ uh and then we'll basically see in the meantime I'm also looking at it so might not be a bounded area issue because the build error should not be coming uh I mean it should not be coming a build error right so it should be a runtime error if the stack is getting overwhelmed And but uh I'm not sure why
 
 
00:20:57
 
Gajinder Singh: it's coming at the build error. So I'll try to also look at it. But yes uh I mean eventually we will have we can't have a stack allocation for uh the SSZ types. So we'll definitely have to move at some point.
Chetany Bhardwaj: Okay. Okay.
Gajinder Singh: So we will coordinate on this and we'll talk with GM and figure it out.
Chetany Bhardwaj: Got it. Okay. Got it.
Gajinder Singh: All right. Uh let's move to Angel.
Anshal Shukla: Uh so I had raised like uh uh four five PRs and like none of them were quite big. So all of them have gotten merged. Uh and they touched up on uh things around logger and uh there was one related to changing topic ID to topic strings and uh another was related to generating message ID uh as per the updated aspects. So uh all of them have been merged already. Uh I think I'll have to pick another uh another issue. Uh I'll go through the issue list and see what I can tackle.
 
 
00:22:06
 
Anshal Shukla: Uh as far as like there are some uh if there's like you mentioned there are some issues that needs to be addressed uh around lip P2P and uh SSZ allocation stuff. So um if if somebody is not working on any of those specific tasks, I can pick them up as well.
Gajinder Singh: Yep, sounds good. Angel, thanks for your contributions. Uh, and uh, hopefully after seeing a little bit of more contributions, we can basically figure out how to work together. Uh, yeah. So, let's move to Noopur.
Noopur Singh: Yeah. Hi. So uh I last week I was working on colored logs uh which got merged and uh now like I have picked up uh refactoring of the code which was a comment on one of the PRs that I raised and I also like created uh uh ZMP PM where all the uh recordings and the transcripts will be available going forward and uh yeah so that's what I am up to I have uh a couple of uh uh like uh topics that I want to discuss.
 
 
00:23:24
 
Noopur Singh: So one of that is like the naming convention that we have uh like the documents that you shared yesterday um so there are uh like the naming convention differs from what we are using now. So uh what's suggested is to use snake snake case and we have been using uh camel case. So I think some of the people have started using snake case. So right now the code looks like a mix of uh uh the naming convention. So we should actually like uh decide and maybe uh like if uh like if I need to pick up and uh like change the code especially for like the uh case change. So I can do that and so yeah first
Gajinder Singh: Yes, we we have uh Yeah, yeah, on that basically I want that you know I so my my liking is that we have uh struct names and
Guillaume Ballet: Yeah, the zig spec says it should be snake case, not camel
Gajinder Singh: function names and uh the function params as camel case and rest basically you know snake case but uh I mean this is my personal liking but we can talk about it uh as such and mostly I defer this to GM and uh we'll go with the style that GM recommends So we'll come back on this I guess uh we'll discuss it and then sort of put down a guideline for Zim to follow.
 
 
00:25:14
 
Noopur Singh: Okay. So, another uh issue was that I have no updates from uh Tamaghna on ricksDb. So, I guess we need to reassign that to someone.
Gajinder Singh: Yes. Either Anshal how good are you in terms of zig bindings working on zig bindings.
Anshal Shukla: Uh I don't have any idea but yeah I'll I I can uh look into the docs.
Gajinder Singh: Yeah. So we have an open issue for integrating uh rox db or level db.
Anshal Shukla: Mhm.
Gajinder Singh: I don't care whichever is easier uh into z so that we can start storing blockchain objects.
Anshal Shukla: Okay. Okay. So uh I I'll search for the issue and then go through the and then I can uh maybe estimate how how long I'll take and then uh we can coordinate further on the group itself.
Noopur Singh: Okay, I'll share the issue to with you, Anshal.
Gajinder Singh: Right. Right. So I mean even though I think uh team prefers rock DB but uh if level DB is easier whatever is easier we can go with it right now and uh basically complete the task.
 
 
00:26:42
 
Gajinder Singh: Cool. All right, we have new joiners. Would you in in the call would you like to introduce yourself? Hurricane.
Hakeem OREWOLE: Um hi everyone I am from um UK. So I joined the call just to get familiar with the project with the hope of jo um contributing to the project. All right.
Gajinder Singh: Good to see you here again. Uh so just tell us uh what what kind of work have you done in the
Hakeem OREWOLE: Yeah, I I am currently like a tech lead for my um for a company I started and um with Zig I was contributing to the Zig implementation of Bitcoin sometimes last year but the project got um stopped. So I was looking like to contribute to another blockchain related project that is written in ZIK. That's why I'm looking at Zim. Yeah.
Gajinder Singh: All right, welcome to Zam calls and Zam uh codebase. Uh all right, we have Wankang Chen.
WenKang Chen: Uh hi. Uh I actually joined a previous call but uh I have not been active but yeah I'm just catching up with things I would say.
 
 
00:28:29
 
WenKang Chen: Um yeah I'm still getting uh used to zig itself. Um I'm still new to language. So yeah I'll see what I can contribute as I get more familiar with the codebase and also like the language itself. But yeah that's why I'm here. Hi
Gajinder Singh: welcome. Okay, so let's move to Ladislaus. Ladislaus, how is it looking from out there?
Ladislaus: Yeah. Hi guys. I don't think I have a like a huge big immediate update. Um, as you know, we we're coming closer to the to the Cambridge workshop which uh still takes some some coordination effort around it. maybe one sort of um I guess a bit future or forward looking uh piece of news is that so we've started talking to um Dan and Alex who are uh previous uh EPF fellows and who have uh in the past been looking into um I guess a subset of what we call rainbow staking um which is called enshrined uh operator delegation. So um I guess it's for for for this group the relevant part is that um like beyond um I guess the initial specking efforts um we see uh we see a decent chance that sort of the the rainbow staking um uh specs could could be sort of the the next step let's say end of year or early next year and sort of after the this is just a just an FYI after the PQ workshop in Cambridge
 
 
00:30:13
 
Ladislaus: We have a another workshop actually also in Cambridge with the architecture team of the EF if uh we have actually invited um both Dan and Alex uh to sort of discuss uh and present sort of their their initial findings. So, uh, yeah, it's it's still very alpha. It's still very early to say where where this direction goes and sort of, um, but but we we'll elaborate, um, more in in October.
Gajinder Singh: Yeah, I think yes. Uh, rainbow sticking seems like the logical next step post pets. So, it's a very good idea to get started on it. Cool. Uh, now we will look at our uh, DevNet zero readiness. Uh so we have uh most of the spec implemented uh apart from us uh transferring into SSZ list uh which is something that we are working on and uh we have the fork choice implemented uh we are we have integrated uh uh the genesis generator and are able to start nodes with it. Uh so we are able to do local SIMs uh with our own nodes on LIP P2P network.
 
 
00:31:36
 
Gajinder Singh: uh we are able to do finalizations uh basically you know at in consistent finalizations if you if you run the beam sim so you will be able to see that we have uh lot of uh uh log improvements through various PRs of Noopur and uh uh from Anshal as well uh recent PR from him and uh so with that uh I think uh it we the next step is for us is to integrate the spec test genesis uh all kind of spec tests that are out there. So that uh we should be able to then start up an interop and for that uh we also need to uh complete our pending task on P2P uh so that we can talk with the ream node and that is something I think uh we should target next week. So the current focus will be to uh have these series of uh CLI tests running in our CI and then have the spec test again running in RCI starting with something and then basically adding a whole bunch of it and uh then uh the next step would be to interop with RAM.
 
 
00:32:57
 
Gajinder Singh: So with that regard I think uh we are at a good position and we should basically double down on our pace and uh take it to completion. So that will be all uh on the devnet zero end and let has already updated you with the the goals beyond pq devnet uh with what we can do. Uh so I think uh is there anything else that you guys would like to cover?
Mercy Boma: Yeah. Um I wanted to ask um the two PRs I raised can I get like a review on it so that it can be merge? I think the one the sim test is ready but also the one of simulation on the genesis and finalization I'm unable to run two notes at the same time with it. I dropped a comment. Kai also dropped a comment on it.
Gajinder Singh: But if you run the beam beam same you are able to I I can always run the two nodes.
Kai Chen: Yeah.
Gajinder Singh: What what do you mean by you are not able to run it?
Kai Chen: Yeah.
 
 
00:34:12
 
Kai Chen: Yeah. Could you could you take a look the comments in that issue?
Gajinder Singh: in which we are.
Mercy Boma: 194 Yeah,
Gajinder Singh: So this is this is the sim that you are trying to run using the node command that K developed.
Kai Chen: Yeah, there the Yeah, there is a hard code uh right now.
Mercy Boma: the the the this thing is hardcoded. So I can like override it to run two notes at the same time to finalization at the same time.
Kai Chen: Yeah. Yeah.
Gajinder Singh: Which VR number is this?
Mercy Boma: 194. It
Kai Chen: 194.
Gajinder Singh: Yeah, I'm opening it up. Yeah. So, why do you need a global swam state? Because I mean the two nodes will swim they will spin up their own lip uh e lip networks and they should talk with each other. It has nothing to do with having a global swarm state right. So they have their own they have their own uh uh rest2b bridge opened up. So you have to open uh two separate processes and that is how this particular PR is different from beam sim because beam sim is doing it in process with the same network uh instance but now there these two nodes they are independent processes and they will need to talk with each other using uh sleep P2P again I I don't see uh Right.
 
 
00:36:05
 
Gajinder Singh: This should be an issue.
Kai Chen: Yeah. Yeah. So right now we must use uh two process Okay.
Gajinder Singh: Yes, both both are using network ID zero. That is correct. Yes, both needs to use network ID zero but their own process. Cool. Yes. So that that is the point of doing the same that now with their own process they will behave exactly like two different nodes right. So that is the point of adding this same in the CI as well. All right cool. Uh so that will be the end of today's call unless anyone else want to add something in
Guillaume Ballet: Uh yeah, just a quick uh thing about the the dashboard. So I'm still ironing out some details with DevOps. Um yes. Uh okay, how do I put this politely? It it's a struggle to to get DevOps to to do what I want. So maybe I'm just extremely uh extremely uh demanding uh or I don't know. They have their own way of working and it's it's not mine.
 
 
00:37:28
 
Guillaume Ballet: Um yes.
Gajinder Singh: You are extremely divine. But that's a good thing.
Guillaume Ballet: Yeah. Okay. That's fair. That's fair. Um and uh yeah. So, uh it's kind of there. Uh simply, uh yeah, I I just need to to iron a few things and uh uh we're just uh working out how I could grant, you know, more people access to this machine. Uh because currently it uses one of their internal tool. Um yeah. Okay. Details. Um but um I think it would be ready. I mean uh you know I could even show what it's doing right now. Uh if uh if you have a minute uh I also don't want to unnecessarily uh you know hold the call. Uh but just for a quick demo uh where is this right? Most uh most of the demo is going to be me trying to find the right window. Here we go. Um, so you you can see like it's the it's the the metrics being uh displayed by a live z node and I have to do the same thing with uh with ream in the docker container and try to try to make this happen. Uh so just FYI it's happening. Uh I I think we agree during the last call that we will define the dashboard themselves in Cambridge. So that's fine. There's no emergency, but just FYI, this is this is happening. Oh, yeah. I need to manually refresh at the moment. It's not it's not a dashboard. It's just the query interface. Um yeah, so just uh FYI, this is happening.
Gajinder Singh: Amazing.
Guillaume Ballet: Uh you'll hear from it a bit later.
Gajinder Singh: And thanks game for choosing DevOps and getting this done. All right. Uh so this brings us uh to the conclusion of today's Zoom call. So we will see you next week on 26th September and before that we will see you on uh the PQ interrupt calls happening on Wednesdays. So basically that will be on 24th September.
Chetany Bhardwaj: Bye
 
 
Transcription ended after 00:40:19

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
