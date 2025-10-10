# Zeam Weekly call - Meeting Notes and Transcript
# Date: October 03, 2025

## Meeting Overview
**Date:** October 03, 2025
**Recording:** 
---
## Meeting Notes

Oct 3, 2025
Zeam call  - Transcript
00:00:00
 
Gajinder Singh: Hello everyone, welcome to October 3 Zoom call and we will go discuss updates. we will also discuss our readiness for the workshop as well as pq-devnet zero and then we will basically discuss any other pending questions or queries that that are there. So with regard to the status let's with regard to the updates let's start by from Anshal
Anshal Shukla: Sure. So, during this week my DB PR got merged and apart from that I also started working on states pointer PR that is still pending and so we have two options. So Guillaume has already merged the changes that I had made on  SSZ but there are some other changes that Chetany had also made. So either I for the time being I can instead of using pointer data types I can simply use like the reference type and get the pr merge and create a separate issue or we can wait for Guillaume and Chetany to finally merge the pr that Chetany has already there on  SSZ and then I can move forward with this PR.
 
 
00:01:20
 
Anshal Shukla: Uh so yeah that's one thing. Apart from that I am also looking into Peer ID when event so when when the a peer gets connected or disconnected. So I have a working pr right now at my local I am I am reviewing it for myself and I should be ready to push it by end of the day today. and apart from that there's third thing where where I had like pushed a refactor of like our types containers and make it more more in line with how it is there in the specs. So that that got a little so there are some merge conflicts on that because it got collided with Kai's PR. So I'll update that as well. I wanted to discuss like should I like make quite extensive changes onto that PR itself because there are quite a lot of things around beam state as well where they where we have like separate functions and state transition package where we we do all this process block stuff and process slot stuff.
 
 
00:02:36
 
Anshal Shukla: So should I like make it more in line with how it is mentioned in in lean spec or should I for the time being so for the timing I've just moved process header onto beam block but I can like update my PR and make it more in line with how it is described in lean spec.
Gajinder Singh: Okay. So I'll basically need to look at that u because apart from the helper functions which were around the state regarding flattening and unflattening the list I'm not sure what else we should move over there as part of as part of you know straight or block structs but I'll take a look at it. also it would be better if instead of having a big PR we basically have small small PRs that proposes these changes so that we can decide on a case by case basis for this yeah we
Anshal Shukla: Yeah, got it.
Guillaume Ballet: Uh, can we please not merge this before the interop?
Anshal Shukla: Okay.
Gajinder Singh: do have some spec PRs that I can try to see and figure out what's going on over there and see if we can merge it if there is there there aren't any structure changes in it regarding regarding
 
 
00:03:55
 
Anshal Shukla: Okay. Okay. Yeah. Apart from that.
Gajinder Singh: your  SSZ thing. Can you merge main into Chetany's PR and then use the latest commit of Chetany's PR into your state pointer PR?
Anshal Shukla: Yeah, I can do that. I'll need access to that particular repo because I'm not added as contributor contributor on SSC. So I won't be able to push directly onto Chetany's PR. Either I can create a separate PR on top of Chetany's PR or Okay.
Gajinder Singh: Yeah, Chetany will give you the access I guess to his forked repo. But Guillaume, maybe we can add Chetany and Kai to if Kai is not already there to the  SSZ maintainers, I guess.
Guillaume Ballet: Uh maybe not now, maybe later.
Gajinder Singh: All right, cool. So for now Chetany please provide access to your fork to so that he can or you basically merge and update your PR I guess that would be the easier thing to do and I think the merge is also available on the UI so it can be done just from there as well.
 
 
00:05:21
 
Gajinder Singh: Yeah Mercy
Mercy Boma: about the specs pair. I think Guillaume did made he left a comment about it which also affects mine. I don't know if you can take a look at it.
Gajinder Singh: Uh, so you have some PR up that you want us to review.
Mercy Boma: Yes, the specs PR um answer talking
Guillaume Ballet: Yeah. So, yeah, sorry. Let me let me provide some context here. I was just trying to give access to check my who who's a collaborator in that in that repo, the SSZ repo. Um yeah. So, okay, there are those lean spec tests, right? Um and I don't I'm still trying to understand how they're working. So, this is why I also said in the comment that I need to double check. Um but the the way this works for execution for example you don't reimplement the spec tests you just get some program that takes a lot of pre-filled tests and those tests will um will take in yeah they like they those pre-filled tests tell you exactly how you should pre-configure the the the the like the client how you run how many blocks you run with what transactions, etc., etc. And and this is how you you do it.
 
 
00:06:48
 
Guillaume Ballet: You don't you don't rewrite everything all the time for every client because if you do this, you're probably going to create some bugs. Um and um and we don't want that. We want just to make sure that whatever is provided as YAML files, which is what I this is why I was asking you for this. I'm trying to to understand what they want to do and what state it's in. Uh looks like it's not there yet. Uh but um yeah, once once this is done, once those YAML files are generated, you just you just run your client against them, possibly with an extra spare executable. And um and when you do this you see if you agree like you you return the the state root and you return some information about your own internal state so that the testing framework can figure out if what you do what you return is um is compliant with what they expect. Um and I think I think this is what we should be doing. So those two PRs in my view should not be merged.
 
 
00:07:53
 
Guillaume Ballet: they should be they should be implementing that client. Now I understand from what I from what I'm looking at what I've been looking at today is that this whole framework is not ready. Um but we should we should try to work on that framework and and help them put it out the door and be ready for it. Uh rather than um rather than reimplement this because it's going to be dead weight pretty soon. Uh I  think we should we we should just wait a bit see what comes out of the discussion this weekend and then um yeah then look at it and either it's not going to be ready anytime soon in which case maybe we implement our own thing but yeah I don't think what we want are the the zik tests.
Gajinder Singh: Right. So, the idea behind this was that Thomas said that it might take him some time because he was busy somewhere else. And so the point was that can are we matching with the spec because if we want to interop with ream or qlean then basically instead of just rushing into interop blindly we at least figure out that okay you know we are matching some of the spec and the main thing to match was state transition and I think that's where the people are doing various PRs to m match different different parts of state transition but uh
 
 
00:09:22
 
Gajinder Singh: So that that was a main point of doing this PR that even if we don't have the spec format ready but we should at least have something matching so that we can go into interop. So yes, this is a transient way of doing things but and the right way to do things would be what exactly you mentioned.
Guillaume Ballet: So wait how do you check that it's matching? Because okay I understand and I definitely value this um this approach if like this transient approach but how do you know that Ze is doing the same thing as ream? Like is is this is are those values hardcoded anywhere?
Gajinder Singh: Yeah, that is what my expectation of the PRs is that they are finally running the Python test and for example hard coding the hash roots and checking them in Zeam as well. So this is what my expectation is but I 'm not sure of the individual PR. The only PR that we have merged as of now is of Kai I think and that was a very basic packing unpacking PR in which we did discover a bug with the lean spec and basically we got that updated thanks to a PR.
 
 
00:10:37
 
Guillaume Ballet: Okay. Sorry. Yeah. Well, that that's cool. Uh but this Okay, maybe I have to go over it. Um but the tests themselves are now running Python, right? They're just looking at what the output of the same Python test used to provide, right? Is that is that correct?
Gajinder Singh: Yes. So basically team member who has a whoever is putting up the spec PR they are thing running Python and they are supposed to get the hardcoded output from the Python and replicate it.
Guillaume Ballet: and the source are the lean specs by Vtoma.
Gajinder Singh: Yes.
Guillaume Ballet: Okay. Okay. Cool. Thank you. So I will review the PRs with that again with that that understanding in mind and if they are satisfactory I will merge
Gajinder Singh: Yeah. And guys if you have not compared with hardcore rate straight root or block route something that basically anchors your spec test then basically that spec test is incomplete because we have not established one to one correspondence with
 
 
00:11:32
 
Kai Chen: Yeah. Heat.
Gajinder Singh: what we have written and with the spec test that is in Python. So if if that is not there please add it so that the spec test is complete.
Kai Chen: Yeah. Yeah. Yeah. So, so the the easy way to do that is add add some print in the Python test print the hash hash tree state in hx. Um yeah. So copy copied in the zim test. We can we can check it.
Gajinder Singh: Right. So do as Kai has pointed out and I guess we can sort of for transient time we can accept those spec test for now because again we don't want to rush into interop blindly.
Guillaume Ballet: Yeah, for sure.
Gajinder Singh: And we did and we did also find an issue thanks to the guys first we are on it. So yeah that way I think it's very meaningful Anshal on the third thing that you mentioned regarding maintaining the connected and connected peers right so so what's the status on that you're saying that it will be available by today evening right because Right now if we we are able to connect the nodes but then we should also know whether they are connected or how many PS they are connected with.
 
 
00:13:38
 
Gajinder Singh: Uh so that information will be quite useful. One of the other things that I think use useful would be to can we have a static status bar on the bottom of the terminal so that we don't have to keep printing our info messages regarding
Anshal Shukla: Yeah.
Gajinder Singh: change status but on the status bar we can keep updating it or repainting it. I'm not sure whether there is a handy tool available to do that in terminal or if there is or if that is only can be done in Linux terminals maybe then we can have a flag which basically says that sticky status or something and curses. Yeah, I think I have used encursives in past but I'm not sure whether we should add that or how difficult it would be to add that but I think that would be cool because then our UX will be quite different from what other clients have and I think it will be a significant improvement rather than trying to see the things that have scrolled by. So maybe
 
 
00:14:44
 
Guillaume Ballet: if if I may suggest something here because encurses you know is definitely available in many places in Unix. Uh for Windows I'm pretty sure it's not available. So what we would do maybe is to have an extra CLI that connects to the node and displays this stuff. Maybe we could do this. I mean it it's just you know otherwise we're going to have to either package and curses or you know or be at the mercy of whatever is in is installed in on the local machine and this is this is a big problem for guest for example like we we try to not accept any any extra fancy features precisely because of that. Um yeah, I mean I 'm all for the fancy UI that that is much better than than any other client. Uh I'm just saying like be careful. Don't make it a requirement because it will not be available on you know it will not be available in docker containers. It will not be available in many places that we don't think of right now and we only find too late.
 
 
00:15:51
 
Guillaume Ballet: Or if you do that, do this after the interop.
Gajinder Singh: Yeah. Or basically add via CLI parameter not even while connecting a CLI so that a person who is running it knows what they're doing.
Guillaume Ballet: can also be a build option.
Anshal Shukla: I can I can look into it. Uh but I think I'll create a separate issue for that. and not merge it with like the current thing because like we sure
Gajinder Singh: Yeah. Yeah. Just just create an issue. Uh I mean if you want to do it, you can do it or anyone else who wants to do it can do it. Uh it's something that came into my head and I just wanted to put it out there so that we don't lose it. Cool. Uh okay. So next we can go to Kai for update.
Kai Chen: Uh this week this week um I do I did some just for spec test spec test task and and some issues in the quick start in quickstart project.
 
 
00:17:10
 
Kai Chen: Yeah. Um and I  also spend some time in zigp I almost finished all the all the features about gossip sub except except the heartbeat. Yeah. So I will I will start to do the publish and the subscribe test next week.
Gajinder Singh: That's great news guy. Yeah, I think we would basically need to chisel a bit lean quick starter a little bit so that we can also incorporate idiosyncrasies of other clients. Uh and I think that would be cool. There are some issues there as well like you know some automating some of the things but I guess we can take it as we go along and but the main thing I think would be to add request response and I have been thinking about it how to do that and to also add parent chain syncing because these are the two important bits that our clients still needs and we need to sort of close this out and this is basically on top of my head.
 
 
00:18:43
 
Gajinder Singh: So continuing on my update so I basically worked to create lean quick start so that we can start cross-client interop and then basically did some changes with mercy regarding the CLI arcs that node commands accept and I think there are few more changes after discussing with other teams but they are cosmetic in terms of naming some of the CLI ads that Kai is undertaking. Uh so once Kai is done basically then I will try to integrate again with lean drop and now that I have got command line args from other clients as well. So I will try to integrate them and try to actually do an interop and see how it's going on. So that is on top of my head and if I'm able to do with the interop then basically I would try to start working on request response and then the next step would be parent chain syncing. So yeah that is from my side. Now we can go to
 
 
00:20:09
 
Guillaume Ballet: Yeah. Um, well, I mean, I've been I've been reviewing a few PRs. I'm still in the middle of this. I'm also because there's been a lot of changes that got merged when I was looking which is entirely my fault. Um and I'm trying to catch up a bit into the with the the current state of the codebase. Um I'll be flying tomorrow to the to the interop Um yeah, that's that's pretty much it. Um I yeah, I mean we we had this conversation about the the spec test, so that's that's fine. Uh I was also preparing some machines to be able to run the to run um well you know validators or whatever during the interop to see to see how they're fairing using linquart another one of those great gender tools. Um, in fact, I was actually trying to argue with with Perry today from expand off that we should give up on kurtosis all together, but I don't think I mean it's it's me trolling of course.
 
 
00:21:24
 
Guillaume Ballet: Uh and and the troll is working. Um I would like to talk to Chetany. U you know, I mean we can do that during this call or later. Uh I  reviewed the PR. Um, yeah, I'm sorry to say I'm not very happy with Okay, happy is a strong word, but I  don't think this merge should go in. Um, so if you haven't seen it already, Chetany, please have a look. because I would indeed like to merge something similar so that we can make a new release and include Anshal's I mean first your changes but also Anshal's um changes and and and include that in the in the main thing. Um yeah, so I'll wait for you to give your update, I guess.
Gajinder Singh: I guess we can then directly go to Chetany.
Chetany Bhardwaj: Yep. So worked on few minor things. Got his unfinished PR and the other one he has already picked it up again and a few fixes. Uh I  I a few issues I found in the open PR just fixed those and I  have reviewed the comments and I  definitely do agree with a lot of things over there that game has left and maybe we can discuss it after this and apart from this the spec test that I'm working on right now.
 
 
00:22:55
 
Chetany Bhardwaj: So if I  I ported it like one to one to our test and the issue that we are having right now is that what the lean spec does is when they are processing a test stations if they dynamically reallocate if the slot u so we have hard check so if the slot number that we have is greater than the justified slots length we return an But what leans back do is the dynamically allocate on top of that over there itself. So the test I was unsure if I should be changing things to get the test to pass or push it right away as it is. But as of now that is the difference that we have from the lean specs processor test stations and ours. Ours has a hard check and it returns an error error right away while the lean spec they dynamically allocate on the go. So there is that and yep so let me just open the PR that I had. So okay it's yes.
 
 
00:24:08
 
Chetany Bhardwaj: So the comments for for the get default one I'm almost on the same page. It it seems that we are imposing a lot of things with that one. Maybe Gajinder can also give his input since he raised the issue. And for the migration from bounded list to array list that one I'm still unsure. I did read Kai also mentioned that in Zig 0.15.1 the APIs for our list have changed. Anyway, so we would need to change things over there even if we go through with this migration right now. But happy to have other opinions and work on top of what GM said as well.
Guillaume Ballet: So to also um maybe make my my review less of a definitive statement, I haven't checked if bounded array still existed in 115 sorry 051. Uh I should do that. Um but look, I'm I'm not trying to stall the thing. If you know, if we just need to replace it later, that's fine, too. Um but but I don't like the concept of bounded array sorry of list array because it's not bounded at all and so we have to you know add this check all the time and potentially miss one and then we find ourselves with bounded arrays being sorry with list not having a limit anymore and that's why I'm fighting a bit to to keep the bounded array if we can allocate it um on the
 
 
00:25:49
 
Guillaume Ballet: stack sorry on the on the heap instead of the stack Um, this hasn't been checked, but I think it should be checked. That is if it's possible to do it.
Gajinder Singh: But but one problem with still allocating it on heap is that you even if your list isn't full you are allocating for full right so and some of the list are going to be huge I mean it might be that okay you know all those list might get smaller as we move on to further iterations of the spec but right now if you allocate for them it will be a Celebration.
Guillaume Ballet: That's absolutely true. Um, but that also means that if you're um if you're not you know if you're having a list that is full, you're going to be allocating every single time and that's um well I guess if we're having a back a backup allocator um then it's not as much of a problem. Yeah. Um I mean okay like I guess it all depends on what our expected consumption is because you know if if the object is like I  know a signature is like 4 kilobytes about just under that.
 
 
00:27:01
 
Guillaume Ballet: If we're having a million signatures then we're having 4 GB of data that's that's a problem. Uh are we going to have a million signatures? Like what is going to be the n in that case?
Gajinder Singh: right right so in that case I don't think we are going to have in fact in fact signatures are not going to be the problem because we will have only one block signature that is one of the PRs that I have proposed and I have not finished that we aggregate all the signatures in the block signature. So there will be just one block signature. The only issue that I have faced over there is that for example if you haven't got a vote that vote directly and you have for example got a voter attestation from an aggre from the block which whose signature is an aggregate aggregated signature and for example that was on one branch and you want to pack that vote onto some other branch which didn't see that vote. Uh so would you be okay including the aggregated signature that was in the block?
 
 
00:28:05
 
Gajinder Singh: Ideally it should work out because the signature itself is just a proof that they are valid and saying that this was a valid signature of this particular what in X block is like a so you can basically construct a proof aggregated proof over all these signatures as well. So in that sense it should be possible but that is something that we need to discuss and figure out. Uh so if if this is the case then probably we will just have one block signature and then it wouldn't be a problem at all. but then we also have justification list which is actually quite huge and again it depends upon how how it will be how the future spec will be because it right now I think it's just
Guillaume Ballet: Okay.
Gajinder Singh: a shortcut way of doing the 3SF devnet but I think it will evolve into a better spec where your justifications list will not grow too much or it will be maintained somehow of the state so
Guillaume Ballet: Mhm.
Gajinder Singh: so right now yeah we have the transient problem of big list for sure but even then I think okay maybe when we don't have big list then we could have bounded array but I think if someone wants to allocate a big list it would still be a problem so maybe we can come think of it later on and remove array list if we figure out that is a bit of a problem later
 
 
00:29:37
 
Guillaume Ballet: Okay. So, let me let me go the other way around. Let's say okay, I don't like every list. I don't like the the bounding sorry, the l the unbounded aspect of it, but let's assume it's properly, you know, there's no bug. Uh we could do this. The only thing I require then is that we do allocate it. Uh ah yeah but that's the same thing because we would allocate it with capacity. Um okay fine. Um let's okay if you don't mind do as I say it's use the bounded list. If it does allocate too much memory, we can we can decide to either pick a different back end, you know, like have another backend type for special cases when we know it's going to go overboard. Um and then let's simplify this PR a bit so that we can merge it and be done with it. And if we realize okay this is allocating too much memory we should we should see it on the on the metrics page then we can we can go for whatever will replace array list.
 
 
00:30:51
 
Guillaume Ballet: Um that would be my okay that would be my preference. I'm not going to die on that hill if you really want to do to keep it the way it is.
Gajinder Singh: Yeah, my my thing was that let's sort of address other issues and merge in this and then if we really want to go back to the bounded area, we can again go back. I mean that was my sort of thinking.
Guillaume Ballet: Okay, fine. Yeah. Okay. It's just that it's going to be a bit slow and we're going to probably let it faster, but Okay, fine. Let's Let's do this. Um Okay. I'll have another pass and I'll merge it
Gajinder Singh: All right. Cool. Uh, yeah.
Chetany Bhardwaj: So I  I  think this one is just up for review. Then the other one the get default types. Uh GM suggests that we drop this and I  mean looking at his reasoning I think that does make sense to me now. But if we could have a review on this as well.
 
 
00:31:58
 
Gajinder Singh: I think so we I'll review this later and we can talk about later as well because it is not that high priority so we'll figure it out.
Chetany Bhardwaj: Okay. Right, right, right. Okay, then and for the spec test one, should I just raise the PR with the failing test for now so that everyone can have a look?
Gajinder Singh: Yes, if there is any test that we are failing on the spec, please raise the PR so that I can debug.
Chetany Bhardwaj: Okay, cool.
Gajinder Singh: I mean, I'm less concerned about the passing PRs. I'm more concerned about failing because if we are failing, then there is a problem that we need to figure out.
Chetany Bhardwaj: Yeah, pip. Sure, I'll raise the PR. Uh, hopefully we pass and everything is right. It might just be that my implementation has some bugs.
Gajinder Singh: All right, cool. Uh, so I hope to see that PR soon. Cool. Okay, let's move to Mercy.
Mercy Boma: Good afternoon. So um this week I worked um I was working on an issue then I noticed a bug on the rocks DB um implementation which I fixed and also worked on Gender with the command usage and currently I'm done with my the Genesis finalization test integration test although I received some comments from Kai on it and I would also like to get your input on that also and another thing is I 'm working on a spec test
 
 
00:33:24
 
Mercy Boma: I've generated the hash shoot but I still need to compare it with the L um specs hash rout to check if it aligns properly just like you said then that is all for it and also um the on the metric side I had um we did I did look at the metrics and I wanted to ask because you did said we should start working on it do we wait now or maybe when the P when the PR moves from draft to standard PR in order to begin um implementing it because I feel If there's any changes and we've already made an implementation, it's just like going backward on something we can just wait and do all at once.
Gajinder Singh: So Mercy, can you take us through the current PR because also for Gim's benefit because he's going to workshop and I think he will appreciate the context. If you take us through then we can just look at it and see whether it's in a state that we can start working on it or not.
Mercy Boma: Okay. Let the the metrics P right.
 
 
00:34:30
 
Gajinder Singh: Yes, the one that Katya says that we should start implementing.
Mercy Boma: Yeah. So there's currently um we have um let me get the link.
Gajinder Singh: Yeah. Can you present and sort of take us through it because that would be easy for us to
Mercy Boma: So currently in the PR we have um the we have L metrics fur metrics state transition metrics and validator and network metrics and although there's still ongoing negotiations on what actually to visualize I think there was a review by I can't recall the name and I have some um suggestion but although it was flagged on including um additional um metrics like f*** choice vote weight total and reorg total. So I was thinking if we can implement this al if you can include this in the matrix also.
Gajinder Singh: Can you sort of share your screen with the PR and we can look through it?
Mercy Boma: Okay, I need to I'm on my phone. I need to go in my laptop.
Gajinder Singh: Okay, I guess then we can take a look at retracing and then come back again.
 
 
00:36:03
 
Gajinder Singh: But yes, we should do some some of the basic metrics for sure so that we can understand what's going on. I don't so so metrics metrics would be the next priority for sure once we have spec test running once we have interop happening then we can focus a bit on metrics and get going on Yeah, I think we can implement these metrics. I just I'm going through them and they look fine to me. Uh let's ignore let's let's ignore the state transition one.
Mercy Boma: Okay. So I
Gajinder Singh: Ignore the state transition matrix and then we can basically implement the other one in the state transition. We can implement lean straight transition time basically total straight transition matrix rather than drilling a bit deeper into it.
Mercy Boma: Okay.
Gajinder Singh: So I guess we can do that but I think that is not high priority. Higher priority would be lean head slot. I think that is okay that we can implement lean fork choice block processing time second we can implement lean testation valid total we can implement invalid also in we can implement lean testation validation time seconds we can implement from that I remember that we don't have testration and block validations functions yet So can someone do a quick PR to add them and one would I think refer need to refer it from the
 
 
00:38:31
 
Gajinder Singh: Python code to see to match the validation.
Mercy Boma: Okay.
Gajinder Singh: So Mercy I think if you can add the attestation and block validation and once you do that then we can basically start adding some of these metrics.
Mercy Boma: Okay, I will do that.
Gajinder Singh: All right cool so I think Rahul is there. Welcome Rahul to the call. Do you want to share anything with us?
Rahul Guha: Hey. Hey. Uh am I audible? Uh yeah. So I  joined in a bit late after quite some time. Um but I'm getting started again. I  did a small refactor PR. Um um so like happy to get some review on it and um yeah, I'm going through all the issues and trying to make more contributions. Thank you.
Gajinder Singh: Yeah, awesome. Thanks. Uh yeah, very happy to see you back again and hope to see some contributions and thanks for the PR. We'll take a look at it.
 
 
00:39:55
 
Gajinder Singh: One other thing I think that I just remember right now is Partha basically send me his work on putting the bindings on signature hash. So I  I think earlier Tamaghna Bhaskar were looking at it and now that I guess they are busy in their college or in the internships that they are doing so we don't have them as of now and I think someone basically will start to take a lead on this on the hash signatures bit. So I just shared the link and if someone can take a look at it that would be awesome. All right. Uh so do we have anything that we want for example GM is going to the workshop. Is there anything that we want to ask or want to get things clarified or want to want the lean working group to focus on to basically you know to get clarity and specs on. Is there anything that we would want to propose for the agenda? So I think there the agenda is sort of clear one is to get the spec test format and to get the spec test going in that format and the second thing is interop for which yeah I'll be providing support and working from here.
 
 
00:41:53
 
Gajinder Singh: Uh so I think these are the two sort of focus areas as per my understanding. Uh but is there anything that you guys would like to figure out long term in terms of how the PQE devnets or how the lean effort should be going?
Mercy Boma: I don't really have um a point to give, but this is like my first Devnet, so I'm kind of like still trying to understand how it's really going to happen. Um I'm kind of like in a loop.
Gajinder Singh: right? Right. So right now we are just trying to do the local node interop and once we have achieved that then probably eth panda ops or someone from the devops would help us spin a public def.
Mercy Boma: Okay, just another question. Um, do we have local machines that will be running this? Because currently I can't really run um heavy things on my laptop for so long. So is there a local machine?
Gajinder Singh: Yeah. Yeah. So, so for DevNet we will definitely get some machine from DevOps but for local we are running on our machines but maybe we can purchase and schedule some machines that people can login and run their you know for basically long runs.
 
 
00:43:34
 
Gajinder Singh: But how long do we want to run right now?
Mercy Boma: I mean, as long as the devnet lasts.
Gajinder Singh: So devnet is like 10 days. Uh but I think before we are there we basically need to figure out the interops between various clients and once that is done then I think we would be at that place then we can probably ask devops for some machines that can be provided to client to test them for a on a longunning basis. Uh or we can spin up our own machines, I guess.
Guillaume Ballet: Yeah. So, I mean, uh, what I do for the interop, I'm going to I just bought a new machine on HNER, a new cloud machine that I'm going to use. Uh, clearly it's for it's for my usage during the interop. I don't want anybody tinkering with this. Um, but we could we could totally create more cloud machines. Uh, HeadNair has some fairly cheap ones. I think like it's like, you know, less than €5 a month.
 
 
00:44:36
 
Guillaume Ballet: It's it's manageable. Um, the only thing is they don't technically like people to run peer-to-peer traffic on it. So, they they could get cancelled, but it's extremely it's extremely cheap. And um there's okay, there are things I won't say publicly in the public in the public channel, but you know, it's worth the risk. Let me put it this way.
Gajinder Singh: Yeah. And the traffic is right now not purely peer-to-peer. It's just some machines talking to each other. So, it might just fly by. All right, then. Uh I guess that's good enough for today's call. Again the focus is interop get the spec test working and resolve any issues and provide support to GM while he's there. Uh, yes to the no.
Chetany Bhardwaj: So about the build issue right that Panda Ops raised. I think we had this earlier as well and when I had a look at it it was it I to me it seemed like it had probably something to do with how we fetch the git version and when it fails we have this build issue.
 
 
00:45:58
 
Chetany Bhardwaj: Uh that is just how I  remember it was earlier as well and I think Paritosh from Pandaops did this. So maybe it might be worth putting this up as well.
Gajinder Singh: I think we can look at it a bit later on post the workshop because for the workshop we can just locally build the image and push to some docker hub for example I push to g1 tech docker hub and it's available the latest image is available over there so if anyone if gam needs an image to be built I can do that and push it over there or game can use a locally built match as well. So that I think we can just focus on it after the workshop. Yes, Kai
Kai Chen: have have we had already had some matrix for memory usage. So we don't run the zim node for a long time. So for devnet maybe so maybe I assume maybe we got some memory issues. So I think the is it's better yeah it's best for us to have some metrics or tools to monitor the memory.
 
 
00:47:15
 
Gajinder Singh: Yes. Right. Right. So basically we as a part of making our client robust we will have a machine which will always for example run a devnet local devnet or maybe once we have public devnet we will all we will have
Kai Chen: Okay.
Gajinder Singh: some machines that will be running public devnet and will be so it will be a longunning and longunning machines and it will also generate metrics for us. So I think yes we'll go over there. Uh right now we are it's a little premature. We first needs to basically get the cross client interop working and then definitely the next step would be to focus on the robustness of the nodes. and we can yes start working on u memory stability and other long-term effects that will come while running the node because at least 10 days node run we need to sort of get there for for public devnet participation.
Kai Chen: Okay.
Mercy Boma: Hey, I think the the metric should be under um our own personal our own client metrics not for the lean metrics.
 
 
00:48:56
 
Gajinder Singh: Uh what do you mean by that?
Mercy Boma: I mean Kai is talking about um memory metrics, right? So I was thinking that should be for our own not the general lean metrics.
Gajinder Singh: Yeah. Yeah. I think yes that those are client centric metrics and yes I don't think lean metrics will focus on that. Uh so lean matrix will just focus on consensus part of it that matters to the consensus and so that the lean spec and protocol can be made better but I think the memory comes over in the part that what is the size of the state. apart from that yes I do agree that lean matrix will not focus on node related matrices which we'll need to focus on and we can basically put them as we need it and definitely we need to run nodes for longer time to basically deal with issues that might come in the robustness of the node
Mercy Boma: Okay, sorry. One last question because it was asked on the group chat about um reorg to tell how important it is for me.
 
 
00:50:21
 
Mercy Boma: I feel like it's important but I don't know for others what do you think about it?
Gajinder Singh: You mean the reorgs? in the network reorg.
Mercy Boma: Yeah. Yeah.
Gajinder Singh: So we had one PR from Scotty put up for that which basically prints out the fork choice tree. Uh if we can basically improve it a little bit then it can help us figure out what are the reorgs that are happening and it will be useful to visualize. Uh I think Rioorgs is will be a byproduct of the current fork choice rules that we have and the 3SF and yes so we will be able to once we so we are reaching that state where we can start focusing on that and see what's happening and again we'll definitely require longunning nodes as well as as well as chaos right not the perfectly running nodes we need chaos to introduce the Rios and see how they are getting handled but it is always a bit it is always a bit harder problem than everything else and if we can have better visualization tools then it's good and I think that PR by Scotty does help us if we can sort of improve it a bit and make sure that the log isn't too big but if we for example have a CLI that for example mentioned that you know a CLI that can
 
 
00:51:51
 
Gajinder Singh: connect and that can show the status as well as draw the state tree. I think that would be very useful. Uh so we we do need better visualization tools for sure. And if we have the visualization tools that are for example that are in in the web format then we can basically use it as a generic tool and provide it as a generic tool to the ecosystem and say that okay it doesn't matter what your node is if you have this particular API that dumps out the fork choice it will basically show or if you have this particular API that provides status it will provide you the status. So I think that will be a good idea where we have a separate CLI that can connect to node or through the APIs and can sort of paint the picture of the current status not only of the node but also of the network in the network. It basically is the fork choice, right? So I think that would be pretty nice to have and I think we can use there is some element or I don't know there is some framework that you can use to spin up local web apps and I think we can we can go that way instead of encurses or something like that. It would be a better way for sure. or if it can be done in grafana. I'm not sure about that. All right guys, thank you for today's call. it was a very productive discussion and yeah let's follow through on various points that we have discussed and provide support to Guillaume while he's over there and make sure that we can do a successful interop with other clients. Thank you everyone. See you next time.
 
 
Transcription ended after 00:54:41

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
