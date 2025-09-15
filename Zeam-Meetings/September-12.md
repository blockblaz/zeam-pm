# Zeam Weekly call - Meeting Notes and Transcript
# Date: September 12, 2025

## Meeting Overview
**Date:** September 12, 2025
**Recording:** https://www.youtube.com/watch?v=4QLHu00Fggs

---
## Meeting Notes
Sep 12, 2025
Zeam call  - Transcript

00:00:00
 
Gajinder Singh: Welcome everyone to September 12 Zoom call and uh here we'll discuss about the updates then our readiness for PQ-Devnet-0 and uh then any other things that we want to talk or discuss about the events or uh anything interesting that is coming up in the ecosystem. So as usual we'll start with Guillaume.
Guillaume Ballet: Hey. Um, you know, I don't have to be uh first all the time. Uh, I mean, I I appreciate it, but um yeah.
Gajinder Singh: You are our star.
Guillaume Ballet: So, actually, oh, okay, now I'm going to blush.
Gajinder Singh: Let me start with you.
Guillaume Ballet: It's a It's a good thing I don't have my uh my camera on this time. I don't know why it doesn't work, by the way. Um anyway, so um I uh yes, not much really. Uh I did create the grafana like Okay, I didn't do it. Uh, ethPandaOps did it. Create the grafana instance. Uh, I am yet to uh make it work, but it's entirely my fault.
  
00:01:02

Guillaume Ballet: I've been distracted by a ton of things. Uh, but it needs to happen today. So, uh, it will uh, it will, yeah, I will uh, I will do that today. Um, so when this is uh, so let me give some information. there is this grafana dashboard that I could uh I will send the address on uh on um the channel uh when it's ready uh and it's something like grafana ethPandaOps.io for some reason. Um, and then you can sign uh most of you have their GitHub uh account uh set up. So, you should be able to to connect to it uh using your GitHub credentials. Um once once you get there uh there's a need to set up the the dashboards. Um so yeah, that's the part that's missing that and the connection to to Zeam. But there's also a Victoria metrics instance which is the database where all the metrics are going to be stored.
 
00:02:04
 
Guillaume Ballet: So what I need to check still is the connection between Zeam and uh that um that thing uh and once uh it starts being fed into the database uh you can start using dashboards to create the grafana uh uh the the very nice grafana graphs I guess. Um, so there's that. Otherwise, earlier this week, uh, I was actually, uh, adding more tests and I found a a quick bug in, uh, in the list, uh, in the list, uh, type like the the Merkleization. Sorry, not the uh, yes, that was the Merkleization. And therefore, the roots were inconsistent with Remerkable, uh, which is the Python library that used to be the official one. Um and uh as a result uh yes I need to make a new release. I was hoping Chetany would uh add more tests but if it's just test and there's no bug um I could just do the release and we can add the test later. Um so yeah when I do the release we need to upgrade the Zeam with that version of the of the SSZ library and yeah and otherwise uh I was actually trying to do a bit of tenning like I still have a couple tasks to do on the ZKVM thing that are not uh crucial but it's it's uh important housekeeping that I would like to to do uh before I focus 100% on the PQ test net.
 
 
00:03:40
 
Guillaume Ballet: Um, so I just need the time to do it, but uh I I just have a lot of outstanding code that I need to to p to push. Uh, yeah, I know there's a couple more PRs that I've already reviewed, but I need to circle back on. If there's a PR I haven't looked at yet, please let me know. Uh, that's
Gajinder Singh: right? So just to ask uh a few points uh Gim just to clarify a few things. Uh so this dashboard is it uh going to be a universal dashboard or we have client specific dashboards?
Guillaume Ballet: Um I mean you know uh the way it works currently for GU and other nodes you do have universal dashboards. You have guest specific dashboards and I'm sure you have specific dashboards for other clients. It's just that I never look at them. Um so we can have whatever we want like we we can create our own dashboards. Uh I'm going to try to create one that's kind of standard. Uh but if you know in the worst case we everybody has their own dashboard.
 
 
00:04:49
 
Guillaume Ballet: We just share the database and that's that's good enough really. Um yeah. So both basically the Yeah.
Gajinder Singh: So let us yeah so Ladislaus I think uh one of the things that we can do different from how these things were done in consensus and also previously in execution was that there was no standard dashboard to begin with and since we had that metrics call and all right we want to standardize all all the things I think it would be very good if you know from the day one itself we can get a standard dashboard that all clients have to basically you know comply to so that dashboard basically all clients have to generate data that it can serve the dashboard and that is what complying it to means so I think maybe we should uh sort of arrange a call and uh uh with again Katya and uh whoever wants to basically you know get engaged in this particular effort that uh we that they come up with we basically join uh decide what are the metrics and then basically they or we together come up with a dashboard and I'm saying they because it's good that you know a third party creates this dashboard as a sort of a UX thing right so because if the clients create the dashboard then it won't be great in UX and everyone will go their own way and uh It's better that you know there is one uh party external party to all the client that
 
 
00:06:23
 
Gajinder Singh: has created this dashboards and uh it basically serves as a great UX uh uh view view where we can understand what's going on in the chain.
Ladislaus: Yeah, I think I generally agree that sort of the that it makes sense to to standardize at least some elements like uh I think like when when I compare this to the early days of like beacon chain clients like my experience was that it was mostly about clients having their own like um specialized endpoints which they would expose to dashboards. So uh on on on all different layers of the stack. So um and and this is I mean to this very day there is a reason probably for the eom that's that's the same right um and so um I guess it's uh the interesting bit here would be like what what from client like what parts of a client should be uh or endpoint should be standardized that we have like a let's say userfacing standard standardized dashboard dashboard in which don't and and I'm sure clients want to
Gajinder Singh: Right.
Ladislaus: yeah want to differentiate to some extents and should probably at some to some extent.
 
 
00:07:40
 
Gajinder Singh: Right. I think most of the things can be standardized. I mean each client has the same amount of metrics. For example, how much is the head accuracy or how much is the source accuracy of target or whether the block was received at the right time before the voting started and all that I mean all or how much
Ladislaus: Okay.
Gajinder Singh: time did it take to process the block and how much time did it take for example we don't have that issue but how much time did it take for execution or when did blobs come right when were all
Ladislaus: All right.
Gajinder Singh: blobs available so all these endpoints are definitely I mean there is whole lot of things that for example I dev for Lodestar and you know uh so these are sort of customized all these things are there in all the clients but uh they are not located at the same places they are not named in the same way but for example if we have that naming standardization uh endpoint is not really a big deal basically you can also standardize the endpoint but uh uh even if the endpoint is not standardized what needs to be standardized is the name of the matrix that you collect and a definition of how you a standard definition of how you collect them so that every client represents the same thing.
 
 
00:08:50
 
Gajinder Singh: I think that is needed and apart from that if we have any specialized uh client side debugging or client side insights that clients want then they can have their own dashboard but I think the main dashboard should be a shared one and maybe we should bring uh you should get that call back on to push for this effort because uh we talked about standardization and this is I think the best way to go for it from day one itself we say that okay this is the dashboard all clients need to implement
Ladislaus: Yeah. Yeah. Let me I'm not sure if the if this group gathers regularly um but I would double check and sort of can would also suggest that then at least we or client teams like Ze and re come up with a as you just mentioned you had like 10 10 metrics which or plus 10 plus metric which appear like easily standard standardizable or are supposed to be easily standardized then we should suggest it to them and And then let's see like if they Yeah. If they can if and how they can maintain it.
 
 
00:10:01
 
Ladislaus: Yeah. But generally agree. Yeah.
Gajinder Singh: Right. Right. Uh cool. Moving to another point, Gim that you mentioned uh that we actually need right now is we do need SSD list uh to work. So I think Chetany, how far are we from adding the test that Gam talked about?
Chetany Bhardwaj: Yes. So, uh I was looking into fast SSG and I found there were some validations that we did not have. So, which were like critical validation. So, uh I I have created those those are the tests right?
Gajinder Singh: But what about adding the validators list for example from beacon? Let me try.
Chetany Bhardwaj: Yes, those tests uh I I have most of them with me. The validator one not added yet the one I asked you about but that is I believe one or two test only. So uh I can add that today itself. Uh but what I worked on was the validation part for uh the decode and
Gajinder Singh: So one of so I understand so one of the thing is basically you know uh you so there are two things that that we test that you know positive cases and negative cases.
 
 
00:11:06
 
Gajinder Singh: So you are saying that negative cases we need to cover more by adding validations but are the positive cases working? So basically is the happy flow all good and good to go.
Chetany Bhardwaj: yes the happy flow I already added the test in the last PR in in its itself uh but the validation I I meant was not for the test but the actual validation so if somebody let's say uh do
Gajinder Singh: So again add
Chetany Bhardwaj: a bit list attack where the bit list structure is not valid in itself the checks we had over there were not uh complete so the new PR adds that and there are more tests for this both positive and the negative flow so that PR is up already and the one with the block body type and validator test that one is uh still ongoing
Gajinder Singh: so I mean once you add so can you focus on the happy flow for the validators because right now what I need to do is I need to integrate that into Zeam because right now we are using a slice and I need to change it to list for uh Devnet zero compliance.
 
 
00:12:06
 
Gajinder Singh: So if we have the the validator list working as expected it's basically the heavy flow then I can integrate it and this is sort of uh one of the last things that are remaining for devnet zero.
Chetany Bhardwaj: Right. So you're talking about the test for the validator uh structs or like implemented validator structs directly.
Gajinder Singh: So basically a so I think we would have uh some block body test in uh in in the SSZ library itself. So in the body basically you add a validators array and do SSZ serialization deserialization merkleization right and uh verify correct.
Chetany Bhardwaj: Right. Right. So just extending the beam block body test for now adding a value. Right. Right. Yes. I I'll do that.
Gajinder Singh: Yeah, if you do that and once you have done basically then I can uh sort of integrate the SS library from there on
Chetany Bhardwaj: Sure. Sure. Sure. I'll get to it. Uh today itself uh I just needed some clarity on the validator thing I asked you yesterday or day before.
 
 
00:13:14
 
Chetany Bhardwaj: So I'll get to it. and more again the happy cases and the serialization and des serialization in general I've checked for a lot of things uh there is also a ze style test uh I believe the name of the test is that uh that mimics what we have in ze the commented out code that uses instead of just regular slices uh these list so that also works but I'll add the validator uh array tests as
Gajinder Singh: Okay. So once you have done that then maybe it would be good that if you would basically want to make those changes in uh in Zeam types itself so that you know in compliance with SS structure right in compliance with Devnet zero. So would you like to do that right?
Chetany Bhardwaj: Uh, yep. I I I I'd be happy to do that. Again, I'll need to do a bit more study around the Zeam codebase. I have a basic idea. I did uh the changes as I said last week as well, but that was Yes.
 
 
00:14:12
 
Gajinder Singh: So we we need these things urgently. So like you know if you are able to complete the SS today and uh so I think by in next 2 three days by Tuesday we we should uh be able to integrate it into Zeam will possible.
Chetany Bhardwaj: Mhm. Right. Right. I think this is doable.
Gajinder Singh: Cool. All right. Okay. Uh I guess that also covers your update. Uh coming to my update. Uh so I was able to implement the entire for choice uh changes that we did for Devnet zero and they are now uh in our Zeam codebase. Uh there are few things that I want to improve for example how the votes are added uh for block building. So those are the things those are the improvements that I want to do but as such we are functionality complete and the second PR is to basically uh comply with uh uh the types uh with state transition changes that you know we merged in Devnet zero. So those that PR is all already up and maybe Noopur is sort of resolving uh some build and test issues around it.
 
 
00:15:30
 
Gajinder Singh: But I think uh that we should be able to merge that and if we do that then apart from the list type basically we should be compliant with uh the DevNet zero spec that means that we are ready for DevNet zero interrupt. Uh which brings us to the issue of consuming Genesis generator files and sort of starting it up. I think Kai you were working on that Okay.
Kai Chen: Uh yeah, I'm I'm looking that issue. Yeah, but I have a questions about the validators. Yum. Uh I I'm not sure how to consume that file.
Gajinder Singh: What what are questions? We can go over here because let's resolve it right now.
Kai Chen: Uh so that uh I I will paste in in the chat. Yeah. So, so what's the purpose of this this file? I'm I don't
Gajinder Singh: So, Basically, if you if you look at what PK posted, let me also go to the long link. Um, Noopur, can you share screen and show us uh PK's Genesis generation link?
 
 
00:17:20
 
Noopur Singh: And yes, just one
Gajinder Singh: Yeah. So basically uh uh Kai this is the input that will be provided to PK PK tool right.
Kai Chen: Yeah.
Gajinder Singh: So it says it says that you know this is the configuration that you want that uh uh so for uh basically you know there is shuffle is round table which round robin which you will ignore and then it is
Kai Chen: Yes.
Gajinder Singh: like uh you know what what are the nodes back so first node is about okay uh ream zero is the name of that node. So these are the nodes and ENR is that ENR of that node and count is that number of validators that will be assigned to that node. Right? So this is the input file that will be consumed and that should also be available. So this is the validator config file that will be available and then if you come down over so so then you will get this output that what are the indices that have been assigned to for example Quadrivium.
Kai Chen: Yeah. Yeah.
 
 
00:18:52
 
Kai Chen: Yeah. Yeah.
Gajinder Singh: So what you pasted right?
Kai Chen: Yeah.
Gajinder Singh: So you will you will consume both the files.
Kai Chen: Yeah. Yeah.
Gajinder Singh: You will read from the first file that okay you know uh you we are for example node zero re we are Zeam node0 and zero node has a particular secret key that is in the validator config file. Right. And it has been uh zero has been assigned 147 as indices because it was it was to be assigned three validators. Right. So, so we'll we'll basically we'll basically read both the files and then we'll say that okay uh that we'll pick our secret key from there.
Kai Chen: Oh god.
Gajinder Singh: We'll pick our ENR params from there and we'll use that and then there is second node GML file that we'll use to build the pure multiaddrs right that we want to connect to.
Kai Chen: Yeah.
Gajinder Singh: So, so we so this is basically what the what the configuration is and you will have to read both of them together or basically all three of them together validators config and then this config as well as node yaml
 
 
00:19:47
 
Kai Chen: Yeah. Yeah.
Gajinder Singh: as well as validators right so validators will just tell us that what are the particular indices that are assigned to a particular node and I I guess before underscore you will figure out that okay this is the indices that you need to run zero Zeam 1 means that there is zero node we have there will be z1 node we we'll have and we'll basically say that okay in the cli param we will say that this is zero node or this is z1 node right so you'll have to also take a parameter that will say that which particular node is this and then you will read both the files and then apply pick the secret key from the validator config file apply that pick the ps from node yaml file to connect to and then pick the indices from validators file so that you can to assign to the validators to assign the validator indices to the
Kai Chen: Cool.
Gajinder Singh: node.
Kai Chen: Cool.
Gajinder Singh: So, so this is what we need and basically uh we need it again by Monday itself because so that we can start uh you know testing it uh with the genesis generation.
 
 
00:21:22
 
Kai Chen: Okay. Okay. I will I'm going to Yeah. Yeah. I'm going to do it uh at the weekend.
Gajinder Singh: Okay, cool. And uh I think the next task related to this is for Mercy because she basically thumbs up on the issue that created. So basically the is the uh thing is to use this uh genesis generation and basically create a sim in the sim test in the uh in the zam repo itself. Right? So that will run in the zci. I think she has all she has already been working on a on a task which relates to running CLI test and then uh this will this same test will get added over there. Do you understand Mercy? This particular task uh you do I need to repeat?
Mercy Boma: Sorry, my little cut off. Sorry. Yes. Sorry.
Gajinder Singh: Okay. So uh the task that created today was about uh using this genesis generator and basically creating a sim test in our CI and uh so basically what you need to do is for example when Kai will have would have created his PR of consuming uh these files you will need need to create a sim test that runs in CI that uses this genesis generator to create uh these files and then run a sim test to finalization.
 
 
00:23:02
 
Gajinder Singh: So basically two nodes will be generated and they will two nodes should be generated and then the sim test should happen uh they should connect and uh in the CI itself and then they should run till finalization happens and that's where basically you will say that the test passed.
Mercy Boma: Okay, thank you.
Gajinder Singh: So this is the extension of your CLI test right. So right now you just did so as I commented in your CLI PR that we don't just want the test of the arcs that we are trying to provide to the CLI we want the test of actually doing a sim run using those arcs right and uh if so you need to basically figure need to figure out a way to not run those test when we say test summary minus minus all or something uh uh or maybe enable them with the additional argument. So this is something maybe you can figure out. Uh but those tests need to run in the CLI. So we'll have another CI job that will basically build and run those specific test and uh then
 
 
00:24:01
 
Mercy Boma: Okay. So if I should get if sorry if I should get you right um I can actually like use another um ar argument so that it doesn't cuz you doing the test the way I'm supposed to do it if I run the um zig build summary um command it takes a longer time so I can um activate another um argument for It's
Gajinder Singh: Correct. So if you if that argument is there then you also do a sim test and uh we'll basically you know add another CI job to do that specific sim test only. But uh if I want to do it on my local, I should be able to provide the ARG and do a SIM test locally as well because most likely this will also become another typical check that a person
Mercy Boma: okay. Okay.
Gajinder Singh: would want to do before they finalize their PR that all the SIM tests are also working.
Mercy Boma: Okay.
Gajinder Singh: All right. Uh cool. So this will get us ready. This will ger is ready for uh for the definite zero and uh what we then need is basically uh spec test running spec test and I'm not sure what is the readiness for that so I'll figure that out but uh once they are available yeah let's see Yeah, it it can be done next week also.
 
 
00:25:30
 
Mercy Boma: I wanted to also um point out that we need to the API endpoint we need it ready to be able to use um June's explorer too because without it we can't Okay.
Gajinder Singh: So it's not it's not higher priority than what we do need to do right. So it's like this is priority zero and that is priority one. Priority zero being the higher priority. uh so we can tackle that next week and I don't actually think that it's quite difficult for it is also quite easy to serve those API endpoints but uh uh because all of right now all of the things right now in the memory so it should be pretty easy to serve them. So what I was saying was regarding spec test and uh once we have the spec test then uh Mercy basically you can try or Kai Mercy and Kai you both can collaborate to uh figure out how to run spec test in the CI as well as basically you know we all will figure out if we fail any spec test or if there is any change in the spec that we want to get done.
 
 
00:26:44
 
Gajinder Singh: I think that is what we'll target for the next week. So hopefully by Monday we should target to be totally definite zero ready. Cool. Uh all right Kai do you have any other updates apart from this
Kai Chen: Uh not not much. Uh I just uh finished the uh replace the uh port to multi address in the uh in the Zeam node start. uh uh and I see several issues assigned to me but I think the high priority is the uh read genesis uh file to start. Yeah, I will uh I'm going to do this
Gajinder Singh: cool. Uh yes after genesis thing is done then I think uh once we are able to pass spec test and also uh also basically run sim test in our CI then the next thing would be to interop with the ream and for that basically we need to figure out if we are able to interop with the re on the lip P2P side if there are any challenges over there and to fix those things I think then that would become higher priority but I guess uh it will be next week because the next priority is uh running uh CI sim test and running the spec test whenever they are ready.
 
 
00:28:24
 
Kai Chen: Okay.
Gajinder Singh: Cool. Uh coming to mercy.
Mercy Boma: Okay, so this week I worked on the CI test implementation and um I got a feedback from Benedict. He did mention that it makes sense for us to keep the CLI wrapper in a different ripple. So I would want to know how I'm going to like how we plan on doing that so that I can implement start the implementation. And then I opened up a discussion about the um API endpoint which is currently not a priority for now. And I also have another question I want to ask how the implementation of client interrupt would be like.
Gajinder Singh: Uh so what do you mean by implementation of client interop? You mean the tools that you mean what what what are we trying to replace kurtosis with?
Mercy Boma: Yes.
Gajinder Singh: Okay. So yeah, so we for if you have seen I developed this tool called loadstar quick start for lodestar. Uh so something like that but not exactly that uh some shell based tool or something like that.
 
 
00:29:37
 
Mercy Boma: Okay.
Gajinder Singh: I don't know we'll figure it out. I think this is something that uh will become relevant in next week. Till then we should not worry about uh uh about the CLI for uh uh for signatures. We should not worry about uh uh you know uh June's tool.
Mercy Boma: the
Gajinder Singh: So basically right now we should just focus and uh get the things that we need to get working which is basically for you CLI uh teams.
Mercy Boma: the same test and the Okay.
Gajinder Singh: Yes. And uh yes and using uh genesis generator and then a sim test with that and then there will be another series of uh uh spec tests that we will need to implement hopefully next week. Cool. Moving on to
Noopur Singh: Uh hi. So uh I have been uh like I was working on the file logging thing and it was done last week. So I picked up some minor CI changes. uh those were done and uh so today I was working on the uh tests for merging of for the um uh for the uh SDF changes to comply with the spec and uh so after that is done I plan on taking the uh passing the logger to the different structs that we Yes.
 
 
00:31:17
 
Gajinder Singh: Got it. And also you have uh the logger color PR as well, right?
Noopur Singh: Yes. Yes. Yeah.
Gajinder Singh: Okay, cool. All right, let us anything interesting you have to share.
Ladislaus: Yeah, I guess not. It's super interesting. Um I mean on our agenda is right now the the the Cambridge like interop event. Um we are it's what is it 3 weeks, four weeks. So it's coming closer. Um we've got um a few more attendees coming for the first part. Um I guess some some ZKVM folks as well. So uh I guess the event scope has a bit broadened from just a client interop to like also like a cryptographer gathering and um so yeah very exciting to see like not not just you guys as as client teams or some of you at least but also like um the the the people in the background the theoretical cryptographers but also practitioners um like push things forward in parallel and yeah that's yeah that's I guess the the main part and obviously exciting to hear um how uh how you guys here make make progress regarding definite zero readiness.
 
 
00:32:38
 
Ladislaus: Um yeah, very exciting to to hear and and see that.
Gajinder Singh: Cool. Yeah, I think uh the Cambridge workshop would be have been would be quite exciting and uh unfortunately I'll won't be able to make uh Guillaume Are you able to make All right.
Guillaume Ballet: Yeah. Yeah.
Gajinder Singh: Cool. Awesome. So, we have also a new person here. Anshal, would you like to introduce yourself?
Anshal Shukla: Yeah, sure. So I mentioned uh uh I am I have been working in this space for last 3 years. So I have like primarily worked with Polygon's PS team. There I have made quite a lot of contributions on the Aragon client as part of their multi-client strategy implemented their PS protocol on the Aragon client. uh apart from that I've also worked a little bit with prism team on designing EPBS stuff uh while I was doing protocol fellowship uh in the cohort 4 so have some experience there as well but uh like my primary experiences includes uh include on the execution layer particularly on the side of things and uh some uh contributions on various folks of G as well so uh I am like uh I I don't have any experience with Zig as such but I'll like look into the open issues and maybe pick one of them and uh just try to tackle it and that way I'll like uh learn the intricacies of the language but uh on the
 
 
00:34:19
 
Anshal Shukla: top of it I saw like little bit of the codebase and it like looks kind of familiar because I have some understanding of C uh and have like worked with various static languages so shouldn't be a hard thing on the language front. Uh if like I I'll I'll look into the issues but if there's anything that needs to be tackled and uh uh it's like beyond the bandwidth of other folks here so I can like definitely look into that as well.
Gajinder Singh: Yep. Welcome Anshal to the team and good to have you here. So Anshal wants to contribute to Zeam and uh uh so the starting point would be basically to again get your hands dirty with Zeam and you know show us uh what uh you what you can do and uh maybe you know pick some tasks that are not uh you know needed in the core pipe for example for definite zero but we definitely need it. So maybe no if there are any good uh any first tasks that are out there that Anshal can try hands on and also sort of gets some experience working with Zig as well understand our code base.
 
 
00:35:39
 
Noopur Singh: Yes, there are a couple of uh good first tasks. I'll assign them to him.
Gajinder Singh: So Anshal, how good is your uh depth and understanding for the consensus protocols?
Anshal Shukla: Yeah. So, uh I I have a fair bit of understanding about like the fork choice rule. uh and like uh I am aware about like the different encoding scheme schemes there and I have like uh kind of uh know uh different engine APIs and uh have some experience uh about like different APIs that are present there. uh but like my primary focus has been to understand the folk choice role and uh stuff around that. So uh I would say like I'm more comfortable there but mostly like I I also have like little bit understanding about various uh validator clients as well. uh if there's anything specific that uh you you are like looking for I can definitely look into that as well.
Gajinder Singh: I mean our for example needs are throughout the entire codebase not just focus on one particular thing but yes you can start with something and sort of uh you know enlarge your scope uh so so just look at the code understand uh uh understand the specs for DevNet zero and sort of understand if you understand uh the lean consensus where we are trying to go I think uh that would be helpful for you to basically get all the context and uh yep let's see yep So,
Anshal Shukla: Yeah, sure. Sure. I'll do that and based on like the uh issues that Noopur assigns I'll try to tackle them and if I face any issues I'll like uh ask my doubts on the channel itself like on the telegram Yeah.
Gajinder Singh: right. So, welcome again, Anshal and looking forward to your contributions and engagement.
Anshal Shukla: Thanks.
Gajinder Singh: Cool. So uh I guess that would be all for today's call. Again the priority and emphasis is to be Devnet zero ready by Monday. So get us there guys. Cool. All right. Thank you and we'll see you on 19th September.
 
 
Transcription ended after 00:38:40