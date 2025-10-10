# Zeam Weekly call - Meeting Notes and Transcript
# Date: October 10, 2025

## Meeting Overview
**Date:** October 10, 2025
**Recording:**  https://youtu.be/hYdwR4VxtMU

---
## Meeting Notes

Oct 10, 2025
Zeam call  - Transcript
00:00:00
 
Gajinder Singh: Hello everyone, welcome to October 10 Zoom calls. This is post Cambridge workshop/devnet zero interrupt. So we have we might have some news from that and then we'll talk about the next devnet which is devnet one apart from the updates that we have for everyone in the team. So for a bit let's go to Kai. Okay.
Kai Chen: Hi and la last last week  most mostly focus on the implementation of lin RPC P2P RPC domain. Yeah. So right now almost almost finished. Yeah. Uh I just I just need some refactoring and clean up. Uh yes. So right right now because we right now because in the rust libP2P there is no default  RPC protocol. So we we just copied copied some code from rim and change the codec part because we we need we need do encoding and decoding in the z in the zig side. Yeah. So and during this task I found the issues about snappy frame.
 
 
00:02:02
 
Kai Chen: Yeah. Uh I already do some fix and and right now I'm just want to add some interop test such as I want to dump some go a go test to our li to make sure our the implementation is correct. Yeah. So yeah that that's for for Zim side and for Z the PPP there is some progress I already make made a publish and subscribe test works. Yeah. So next step is to implement the heartbeat mechanism. Yeah. Okay.
Gajinder Singh: All right. Thanks guy for the update. and yes we need to basically we should include some test from a third party tooling like go so that you know we have the coverage of the issues that we have seen in snappy frames so I think once you are done I guess you will patch up the main library along with the test cases yes all right cool And yes so I think once we have request response
 
 
00:03:34
 
Kai Chen: Yes.
Gajinder Singh: implemented then we can basically move to parent chain syncing which will sort of make our client quite robust. Okay. So now we can move to Mercy.
Mercy Boma: Hello, good afternoon. So last week I did some I worked on the le quick start implementation. I did the genesis integration and automated quick stop  in detection. I also started an implementation on attestation process for the metrics we are working on. I opened  a PR for all the metrics that is related to state transition function that Katya  listed and although I'm waiting for a review on that and also on the implementation static process PR that I created and one last thing is I think my PR for the  add to note is ready  so if I can get a review on that will be
Gajinder Singh: Yeah. Uh so first of all, yeah, thanks for taking the lean quick start to the final point where basically you know we have everything integrated including the genesis generator.
 
 
00:05:08
 
Gajinder Singh: Uh so if you are using lean quick start you will basically see a very different kind of a UX experience and I think it is quite good right now and we have re and kine integrated into it. I'm not sure whether the images that we are using in the docker build they are updated to the ones that they have fixed in the interop. Uh but I think it should all work out. So maybe we should ask maybe we should ask Ken and Dream what are their latest images that were working in the interrupt.
Guillaume Ballet: Uh so if I mean I can already answer that they were using their main release thing.
Gajinder Singh: Uh so
Guillaume Ballet: So if you look at whatever was published let's say in the morning of last Monday October 6 that should work. So you can you can pin that that thing for both  and you can use the ETH panel ops one for Ream for clean they they did not yeah they started by the way the interops calling themselves qlean and by the end of it it was called clean  we  I'm not aware of them publishing it on ETH ops but I can give the I can give the what was it called the thing I was using the the repository or the the image name I was using.
 
 
00:06:38
 
Guillaume Ballet: And if you look on Docker Hub, you can see when you know October 6 in the morning or let's say noon, that should
Gajinder Singh: Awesome. So maybe you can collect these images and do a PR to update them.
Noopur Singh: Sure. Uh can we also have like a I don't know. I was not there in the last call. So if like we can have a demo of the quick
Gajinder Singh: Yeah, maybe we can build a video demo and sort of publish it out there as well. So that would be good. So maybe you can do this and if you need any help, I'll be able to guide you.
Noopur Singh: show. Okay.
Gajinder Singh: Cool. Uh okay. Uh we can now move to Chetany.
Chetany Bhardwaj: Uh hello everyone. So this week I did quite a few smaller tasks. So from telemetry and the quick start a few things over there. So for the telemetry thing one thing I did an extra part from what we have in lean metrics was add granular metrics.
 
 
00:08:07
 
Chetany Bhardwaj: So the one that I did for now was the invalid attestations. So apart from just keeping a total count, it will also have a count for each failure. So why exactly the test station failed and it is something I would also like to put out for everyone. Uh especially since Mercy is working on a lot of telemetry things. If we could have granular metrics, this would help us in the longer run as well. So and apart from it worked on some minor changes on the SSE side. So bit list sorry lists of bits are now being handled as bit list. So and also I was working on making the SSC more performant. So there is another good thing I found out. So sorry so sorry yes so there was we were earlier using while DC serialization for bit list which is one of the most used SSG types we have. we were using loops so it would loop out to find the leading zeros so that I found out some directives and built-in functions so we we just use a built-in function so this should be magnitudes faster for that specific purpose trying to find out more things and to make the sec really good and performant probably better than what is out there so these are the major updates from this week.
 
 
00:09:41
 
Chetany Bhardwaj: Also, I have a few PRs that need review. If anybody could just review them sometime this week, it would help me move forward with a lot of things.
Guillaume Ballet: Uh yeah.
Gajinder Singh: Uh Yeah.
Guillaume Ballet: So about this, I looked at your PR for bit lists. Uh the one  the one that is not using bit list actually that is using a slice of bool.
Chetany Bhardwaj: Yes.
Guillaume Ballet: Um I don't I don't think we should do this.
Chetany Bhardwaj: Yes. That's a good
Guillaume Ballet: U this is why bit list exists. So, I'm not sure why you need to have this, but I think if  yeah, my recommendation would be to use bit list instead because this is exactly what it's been designed for. And from what I understand, you're just copying the stuff from bit list into into that specific u use case. Um so yeah I just wanted to discuss with you like I don't understand the context of this but I think we should just say if you're using a slice of bulls just panic with a or just return an error saying use bit is already the behavior.
 
 
00:10:46
 
Guillaume Ballet: Uh yeah, that's my opinion.
Chetany Bhardwaj: Mhm. No, I totally understand. There was this open issue about this to just for to make sure that we have completion to add this and the idea is to just have the bit list like operations on the go. So if somebody passes slices of bool mistakenly and does not use native bit bit list type. So again I'm fe happy to have feedback but this was just an open issue that I picked.
Guillaume Ballet: Yeah, I saw the issue, but once again, I don't understand the the context, so maybe I'm missing something important. Um, but I think that if someone mistakenly uses a slice of bulls, uh, you should return an error saying this is not what you should be using.
Chetany Bhardwaj: Sure.
Guillaume Ballet: And just above the returning that error, add a comment saying use bit list instead. This is what it's designed for and, uh, it will be much more efficient.
Chetany Bhardwaj: Sure. Sure. If if that is the direction we'd like to go in, I'll close the issue and do the updated PR with just the optimization.
 
 
00:11:52
 
Gajinder Singh: Yeah, I think it is better to keep things simple rather than have convoluted logics where you know things magically get swiped in or so it's better just to throw error and I agree with KM on this part.
Chetany Bhardwaj: Okay. Okay. Sure. Uh if if I remember this issue was raised by you. So maybe can you have a look as well? Maybe I mistook it for something else. The issue just said handle slices of pool like bit list.
Gajinder Singh: Yeah, I think it might be from the time when we were basically using slices as the type instead of list. So it must have mixed up with that. So I don't think the issue is irrelevant now. So but this kind of handling is definitely we need an error to be thrown so that people will not use list of bulls instead of bit list.
Chetany Bhardwaj: Okay.
Gajinder Singh: Okay.
Chetany Bhardwaj: Okay.
Gajinder Singh: Okay.
Chetany Bhardwaj: Sorry.
Guillaume Ballet: Um and if I can add something on the more mentoring kind of level we are not infallible.
 
 
00:13:01
 
Guillaume Ballet: Sometimes we do things mechanically sometimes you know we've write an issue because the context like the bit list did not exist. So okay I understand why that could that would have come to be. Uh but it's okay to question what we say.
Chetany Bhardwaj: Thank you.
Gajinder Singh: Right. Right. So definitely I mean whatever your whatever queries you have and if you think that something should be addressed in a very different way please feel free to bring it up and discuss rather than just take it as a general directive of how to do things. All right. Uh, okay. So, one thing I wanted to say regarding metrics was that right now we have one common metric instance across across the entire codebase across the client and we also try to run instances of the chain in the especially when we are running the test. So I'm not sure whether you know having a client wide instance is a better way to go or to have a matrix instance that is being passed around.
 
 
00:14:24
 
Gajinder Singh: Uh so that is something that we can probably discuss post once we have gone through the updates. So I just wanted to mention it's because matrix was being discussed. Okay, cool.
Chetany Bhardwaj: Okay, I had some suggestions on this. I'll go it at the last
Gajinder Singh: Let's go to
Anshal Shukla: Yeah. So I worked on so I had a pending PR in which I was moving state stack to a pointer and I updated it once like GM had released the release of SSC lift. So I updated my branch and it eventually got merged. I just about 50 days back. apart from that I was al I have also worked on adding adding these matrix about connected peers and whenever there's a event that is being fired about a pier being connected or disconnected so I have added these matrix into the nodes and there was one issue that was there while we were trying to run run the beam command on our binary so yeah it took me some time to figure that out but it was like a basic erase vendoration where the network was getting initialized and and the connection was h happening before like the nodes were getting spun up.
 
 
00:15:50
 
Anshal Shukla: So that's why it was not capturing that event. Uh apart from that I also looked into like there was another refactor that was there I have like taken I have merged main into it so that it can be ready it is ready to merge. I think like putting in the hash that view it as well. Uh apart from that I was also looking into this hashing hash signature stuff. Uh didn't spend much time onto it. I think like parts is already here. So he can describe like what are the differences between Rust and Zig implementation and if there are any missing parts on that front and yeah I can start working on working on implementing that shouldn't take much time initially I thought of it like a longunning task because I had to port the rest implementation into this but since like the implementation is already present I can  I can try to port I can try to integrate it and if there are any issues I directly we are on the implementation
 
 
00:17:04
 
Gajinder Singh: Yeah, I think we will discuss this as well whether you know we want to have right now a total zig native implementation which might be missing optimizations or you know we just do bindings for the rest library. so that you know we have parity with what what is out there for definite one. So with regard to that I think in short term we don't want to spend too much time figuring out the optimizations but to just get things going for devnet one and if basically creating bindings for the rest library for devnet one I think we should just take it as our task and then we can also discuss how to move forward on the native work that para has done and Tamaghna or Bhaskar might have done and I don't know but the status is on that I don't think it's complete anyway so with regard to completeness we'll we'll come and discuss about Partha's work on this and see how we can take it forward but definitely eventually we do want zig native libraries so that's a very good direction but So, so for definite one, I think we should right now just integrate the rest library with the bindings not totally ported because it would also put you know pressure on on the EF crypto teams that they will have to review the libraries in time and
 
 
00:18:50
 
Gajinder Singh: I mean basically it's creating more work for them as well which might not be what the time which might not be the right thing to do given the timelines.
Parthasarathy Ramanujam: And I agree with you. Just giving you an update from what I heard Benedict last week. He said he was busy with the Cambridge workshop, but he did promise to come back to me on with his feedback this week. that is sorry next week and he and Tomma would be looking at the code on a high level. He just gave me some feedback but there was some confusion on the parameters being used but I think now the hash library is pretty much has the same implementation of rust. The only difference is key generation and rust is a lot quicker than that of the zig implementation but signing and signature verification are pretty much the same. Um so  I mean I'm waiting for Benedict to confirm that the cryptographically the logic is correct before I can as we discuss transfer ownership of the report to block pass stuff and then continue from there.
 
 
00:19:57
 
Parthasarathy Ramanujam: But Bing also had a look at the code and he also said he'll help me out with some optimization. But I would appreciate anyone I understand it's not immediate but in future if you any of you could spare time another pair of eyes looking at the code would really help. Uh I'll continue to do what I can to achieve the same level of performance but anything any help from you guys would be really appreciated.
Gajinder Singh: Yeah, in terms of zig optimization, I think Gam can definitely, you know, help us over there. And yes, so basically over the longer run we would definitely want to have the entire thing native in ZIG. but if verification and signature generation is already on parity both in terms of the implementation as well as performance then we can definitely use this because signature because the private key generation is totally independent part and we can use rust libraries over there and in our main code code base we can use the zig native implementation.
 
 
00:21:13
 
Gajinder Singh: So I think until if you basically go through it with that regard and see you know we we are having the same performance benchmarks then probably we can definitely do it and not spend time on the bindings
Anshal Shukla: Yeah, sure. I'll I'll have a look and then we can discuss it in the group itself.
Gajinder Singh: game do you have any opinion on
Guillaume Ballet: Um, not really. I mean, yes, I do. I think having a zig library, even if it's not super performant, uh, is not is is better than having a rest library. I mean, you will see during my update that why why I'm I'm saying this. Um but  given the fact that you know it's also not I would I wouldn't scramble to optimize to be honest simply because at the at the interop was getting really worried about the time it takes to generate the the keys and I think there's going to be some push back like it it's cool for researchers to to have a scheme like this.
 
 
00:22:31
 
Guillaume Ballet: I mean it's a good scheme at least it it takes all the boxes we we need in terms of of security. Um where it's not so great is you know people people haven't really thought of the the usability aspect. Um, so I think it might be worth not worrying too much about this scheme because I don't see it surviving very long or at least surviving any review by ACD. Um, ACD being all core deb. Um, so so yeah I think it's fine if it's not the the best of the best. This being said, if there are some cheap optimizations we can do, sure. Yeah, let's let's try to have a look at that. Um, but yeah, it's like I said, I'd rather have something that works and doesn't take forever to compile than than a big Rust library that is supposedly faster, but but yeah, it drags us down because Rust is not as fast as Zig, first of all, and because  because we, you know, we don't have the real optimizations, but those optimizations can change.
 
 
00:23:49
 
Guillaume Ballet: So, I'm not I'm not going to worry about this.
Gajinder Singh: Uh so on this how of is the performance of the zig library with regard to rest on signature generation not on signature generation on the key generation
Parthasarathy Ramanujam: So on signature sorry yeah keypad generation  for say two lifetime of two part 10 rust generates the keep pair in 1 second but the zig implementation takes 50 but on sign and verification it's all sub-second I think 08 millisecond or something like that is the Okay.
Gajinder Singh: I mean that is 250 is quite often kpa generation.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: Uh but yeah okay I mean if the signature and verification is same time then we can consider it.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: Yeah.
Mercy Boma: I did look into it. Um there was a significant changes in like time differences between signing and verification. So I wanted to know what's your what what's the machine you're using to which machine did you use for the
Parthasarathy Ramanujam: Uh I was testing mine on M2 Mac.
 
 
00:25:00
 
Parthasarathy Ramanujam: U the there is a benchmark repository that's available and I'm not sure from when you did the tests. I pushed the latest Rust compatible version yesterday. Uh so if you could probably use the latest one on there's a GitHub workflow report that generates the performance benchmark of zig for different operations key generation signing verification all of that you could have a look at those reports as well and let me know if it's comparable on your machine as well.
Mercy Boma: Okay. Okay. Okay. When I did it was on Wednesday.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: So can we have a CI job that basically generates the benchmark for zix in that sense that you know I'll just in terms of the machines that are running it
Parthasarathy Ramanujam: Yeah. Yes. I do have a separate benchmark report that does the same comparison, but I've run into some issues. I have not corrected the issue because the the number of keys generated I mean the problem with the rust implementation is their ideal configuration that they want us to generate the key is is with lifetime of two par 32 but passing in a customized lower lifetime parameter that isn't directly exposed out of the crate.
 
 
00:26:27
 
Parthasarathy Ramanujam: So what I'd have to do is create a custom version with all the parameters defined there and then try to generate the keys on a rust implementation. So the benchmark comparison between the zig and rust is a bit cumbersome. I want to tidy up that push it to the benchmark repo and then probably we can  do a proper like for like comparison  and then we we'd have more data. Uh if I mean I know all of you are busy. If there's anybody else who could probably help out in this regard, that would be great. Uh if not, I'll let you know as soon as I have that function.
Gajinder Singh: Understood and I think yes with regard our focus would be with regard to devnet one and basically to get things going on for devnet one and I don't think there is much time because there are
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: so many devnets we had by end of this year so just with regard to that I think yeah we'll have to focus quite a bit on just getting things out of the door rather than trying to optimize.
 
 
00:27:33
 
Gajinder Singh: Uh but yes if someone wants to look at it yeah please please feel to feel free to do so but at your own time but definitely Anshal once you have reviewed Partha's repo then we can basically take call on how to take this forward especially if the signature and verification part they have the you they have comparable performance.
Anshal Shukla: to that meaning the
Gajinder Singh: Yeah. So I guess it would be worth to first measure the comparable performance for both Rust and Zig signature and verification and as well as Supa are there any hardcoded test cases that are same across both the repos that your repo has so to establish that they are on the same parity.
Parthasarathy Ramanujam: Uh so that's supposed to be part of my benchmarking repository.
Gajinder Singh: Hey,
Parthasarathy Ramanujam: I'll I'll push that and share that in the Zim Telegram group as soon as it's ready. So my I mean the most obvious test case is to generate the keys for the same lifetime and parameters on Rust and Zig and compare the public key hashes to see if they match.
 
 
00:29:02
 
Parthasarathy Ramanujam: In that way we can be sure at least they're generating the same. At the moment I just compare the format and the the time but I need to dig deeper to verify the hashes also are correct.
Gajinder Singh: Okay. All right. Uh so I think there is still quite a bit of work that is needed on that repo and I hope that you know you keep pushing it so that you know it comes to a stage where we can then consume it. Cool. Uh, okay. So, uh, we'll go with no
Noopur Singh: Okay. Hi. Uh so I last week I like I was up like I've generated one PR for updating the log file path to data directory cli and currently I'm working on writing the finalized and un-finalized data to DB and in between I also picked up implementing the metrics validator metric but it was postponed because of the metric structure which which we are going to discuss later.
 
 
00:30:22
 
Noopur Singh: So that's that's all from my side.
Gajinder Singh: Okay. Uh and I can go with my update. So I basically helped GM in in the interop for various things as well as worked on lean quick start coordinated that and basically then next let's hear from game how how the interrupt
Guillaume Ballet: Yeah. Uh, well, it went well. We we do have interrupt. Um, so yeah, thanks for everybody who was supporting me during during well and before. Um, I found I found a lot of a lot of issues that other people had that were already solved on on our end. In fact, I think we didn't have a single issue. I know I know Kai committed something to the to the main branch. we might actually have had one issue that that got fixed. Um but but yeah, compared to to the other clients, we were by far the most ready. Um so that's good. Um otherwise in terms of discussion,  yeah, there there were some discussion about the peer-to-peer structure.
 
 
00:31:46
 
Guillaume Ballet: So Raul wants to create a specific a specific network for for the link chain. Um yeah, so we were we had a few discussions with this. Um there were a few options. I don't I don't think or at least I don't recall there was a single  decision on this but but yeah like there's there's some interesting ideas being floated around and I think this is one of the of the best selling points for the entire lean project is that if we can demonstrate that we can have a better networking layer coming out of this  this could this could be very good for for selling that case to to the whole of ACD saying look we have a much better performance and even if the whole lean structure doesn't doesn't pan out at least there will be a lot of learnings so in terms of funding for for teams and and things like this it's it's a very good thing  I was leading was co-leading actually with with Tomma discussion on the on the like bindings what what Justin called bindings but it's really h how can we make it easier for non  non rust clients to to integrate all those features  there's been a lot of strong  characters in that in that call in that
 
 
00:33:16
 
Guillaume Ballet: discussion u with very different opinions so no the discussion remains civil but clearly there are a lot of different point of views Um, so I think there's there's a few conclusions for us. Uh, one of them is that we're going to make one and I'm currently working on that. It's it's a lot harder than I expected. U we're going to build a single executor perver so that you will have a a risc0 zeam, a zisk zeam, an openvm zeam etc etc. Um and and this this will be packaged together so that you can decide that if you want to run Zim with your with res zero with your risc0 approver or if you want to verify risc0 proofs or things like this  this will be this will be the way the way it's built and and that's quite good because this way if we have a VM that disappears or for whatever reason is not ready to do an interrupt ZKVM is not ready for for the interrupt. Uh we can easily just not run it.
 
 
00:34:26
 
Guillaume Ballet: Um and that's going to give us a wide variety of clients that can interrupt with with other clients that don't have that don't have this. So I would say it's a bit it's a bit bad in terms of usability because you have to pick your your executable but it's also great in terms of the binary size u because just having openVM and risc0 the binary was ballooning at almost 3,000 megabytes sorry 3,300 megabytes  but yeah so not having that you know the binaries are now I'm the compiler can do a better work of optimizing. The build takes a lot less time. Uh at least the build for a single for a single target takes a lot less time. Um and I think like the the binaries now are like 20 megabytes. So yeah, a lot a lot more performance in the compiler. because yeah when I merged in OpenVM I realized that the the build was becoming so long even when the Rust libraries were built already.
 
 
00:35:39
 
Guillaume Ballet: Uh linking was was really bad. Um so not having to do this is a great thing. Um, as a result of this discussion, I still want to push for more Zig native libraries. Get rid of the rest as much as we can. the day we can get rid of lip P2P and potentially XMSSS and all those the the better we will never be able to handle to get rid of of of that for for Rust for sorry for ZKVMs at least not in the foreseeable future unless we implement the verification for each of them in Zig which would probably help Um but yeah, I would not I would not expect this to to be available for for a long time. Um so yeah, unfortunately we're going to be stuck with Russ for a long time, but at least I think we we need to push for for Zig stuff now and that means lip P2P as well. Uh so thankfully we have Kai working on this and Bing as well.
 
 
00:36:50
 
Guillaume Ballet: Um but yeah, it's I think it's getting it's getting more urgent than I initially thought. Um what else? Um yeah, I mean I'm trying to remember the discussions that that happened. I mean it's mostly those I participated because there were two tracks and and we spent a lot of time debugging the the devnet. Um oh yeah, so the metrics as you might have seen nothing happened. Uh that was not a priority. we were trying to get interrupt and yeah while while metrics are are important I think it was not important on those three days  so yeah I think we're going to we're going to push on this now especially for DevNet one it would be interesting but we decided to leave this one on the side for now  that's all I remember if yeah if you feel I'm missing something I'm happy to to recall but from from what I remember that's pretty much it. U but overall that was a very good interrupt. I think we we achieved we achieved a lot not only in terms of of work and and code but also of interaction and mutual respect with the other teams.
 
 
00:38:07
 
Guillaume Ballet: Um lots of good conversation with Unavood with June with Camille from from Quadrivium.
Gajinder Singh: Cool.
Guillaume Ballet: Um, so I think we we can build on that for for future devnets. Uh, yeah.
Gajinder Singh: And thanks Gam for being there and pushing our agenda and basically taking us to a successful interrupt with other teams.
Guillaume Ballet: I mean, thanks to you, you know, I just deployed the code and thankfully it was working well enough. I mean, I did spend some time debugging, but that was, as it turned out, the the bugs were in other people's code, so that's even better. Um, so yeah.
Gajinder Singh: Yep. And thanks to everyone for their contributions and to make Zim have a successful interop. So that was pretty great. Uh one thing GM I think that we could talk about was was there anything that happened on the spec because right now we have different spec PRs which are basically you know trying to replicate the test that are there in the Python code but I don't think that is the best way to do it and I don't really want to merge a whole lot of PRs just hard coding the spec code out there.
 
 
00:39:37
 
Gajinder Singh: So I also see that you have some in draft spec PR. So is there any progress in that
Guillaume Ballet: Right. Yeah. So that's that's what I forgot. Uh okay. That's one of the things I forgot. Um yeah. So the the spec PR has the you know I started writing this this piece of code on the on the Monday when after we got interrupt everything was was covered. Uh I looked into the the current PRs that they have. Uh I'm not talking about Thomas code because Tomas code is more focused on the on the test themselves but there's the way you interact with with the clients and I'm thinking more of a model like coming from being an EL client dev. Um they want to build this framework which by the way is not complete.  where eels where you write your test and then you do a process that's called called filling where eels eels being the Python client that the testing team is maintaining you know execute those tests and pretty much create the some JSON files that describe the test like what input do you expect what output do you expect  and then you take those field tests and you start feeding them into each client.
 
 
00:41:03
 
Guillaume Ballet: Um and and so this is the way it works or this is the way it should work on on the the E side and and the way it will work hopefully soon. Um they want to do roughly the same thing for CL and they want to do the same thing for for lean. Um the problem is Philippe from that team has written a PR but it's as far as I understand unfinished. Uh what you have to know is that this team is while while being full of good people is undersized compared to the the amount of task that needs to be to be done and  and yeah  they're they're doing a good work but you know there's just everybody everybody needs their time and they have they try to answer everybody and that's that's a problem because usually you interact with them and then they don't come back to you for a week which is currently what's happening. Um and you know this is an experience I've had working on ver working on on binary trees and everything.
 
 
00:42:16
 
Guillaume Ballet: So not not trying to blame their them at all. Uh but the fact is while this is a cool tool it's still up up and coming they they have a lot of restructuring or refactoring going on.  they're a bit under the under the water line right now and and yeah, just adding extra stuff is is not helping. So,  I think on this one I'm going to let let it sit out for for a few weeks simply because they want to, like I said, they have a big refactor that we know is happening. So, I hope they're going to solve that and that will free some resources. Um, but I did write what I thought would be the way for us to ingest those tests, except those tests don't really exist. Um, actually I could generate them from Philippe branch, but I also know it's not going to be the final version. So I guess yeah, this this thing exists. Please have a look. I don't think it's going to be merged soon enough that we can't you know run those tests as we have them but it is the it is the the right way to do it and so I would want to spend more time just making sure those tests can be consumed even though the final the final thing is not quite ready then creating more tests that will be deleted well okay hopefully soon.
 
 
00:43:52
 
Guillaume Ballet: Um although you know it's a bit difficult to find out because it could last two years. It could last two weeks. So if it's two weeks it's not worth the implementing the test on our on our end. If it's two years yes it's definitely worth doing this. So, I guess we need to make a decision. And it's not necessarily my decision to make, but I'm going to chase Philippe a bit to to ask him where yeah, where where they are and how ready that would be.
Gajinder Singh: Yeah, I mean if we can somehow help this make into two weeks rather than two years that would be sort of great and it would be a great contribution from our end as well in the entire lean specs. Uh, so
Guillaume Ballet: Yeah, sorry. I thought of doing this, of course, but  where where I'm a bit cautious is that there seems to be two visions into how to implement this. There's Philippe and I forgot the name of the other guy.
 
 
00:44:54
 
Guillaume Ballet: Uh but yeah, I would like them to sort it out first before we we do help them. And rest assured, given my experience working with them, we will probably have to help them out because they they just don't have enough bandwidth for that.
Gajinder Singh: So is Is it possible that they can sort out this vision by next witness? I mean do we nudge them or what what is the way we can basically in the next witness
Guillaume Ballet: I can ask. I can ask. I would honestly not expect that for one second, but yes. Um that's it. my camera. Um, I would not expect I would not expect them that to happen like it's okay. In my experience with them, it has never happened that fast. Um, I will ask maybe we can at least slap something together, but um, yeah, it's not next Wednesday. It's not going to happen. Oh, okay. I hope to be wrong, but uh, I'll hope for the best, but honestly, it's not happening.
 
 
00:45:56
 
Gajinder Singh: Okay, even if we basically, you know, we get just one test case out there that is for this and that involves state transition, I guess we would be fine. But yes, the major thing would be to sort of agree them on the format, right?
Guillaume Ballet: Yeah, I mean it's a lot of very intricate Python code that is being refactored. So just the time that is going to take us to understand and you know trust me I've been sending cloud code on this one for for months and months. Um it gets well it's definitely better than me but it still gets confused and doesn't do the right thing. So  we're going to need a lot of guidance and assistance but like I said let me talk to him. Let's see what he says. Maybe we can at least focus on getting his vision and that's good enough for us. But but yeah, I wouldn't cancel it.
Gajinder Singh: Okay. Uh so at least let's bring this topic up in the next wet recall so that we can sort of generate some sort of a peer pressure for things to get resolved and build some urgency into it.
 
 
00:47:11
 
Gajinder Singh: Cool. Uh okay. So now I think there are two things that we wanted to discuss but we already have discussed how to go forward on the signature hash repo by para. So we can discuss the other issue which is basically so what what should we do? Should we have different instances of metrics being passed around or what we currently have which is basically a single instance that code paths keep on updating game. What do you think?
Guillaume Ballet: Oh. Um, sorry. I guess I misunderstood cuz I thought that was that was for intended for mercy. Um, so, um, yeah. Okay. Um, I didn't quite understand like single instance of of code path.
Gajinder Singh: So, so we have you know metrics right? We have the metrics which are like the global metrics and they get initialized and then various  you know modules in the codebase in the client basically they call a global function that update that global instance of the metric but we for example have a beam simulation where we run two instances in the same process and pro and basically they will they will they will be overriding each other's metrics.
 
 
00:48:30
 
Guillaume Ballet: Okay.
Gajinder Singh: So I mean do we really care about the beam sim or do we not really care about it because that is the only thing that basically you know we spin up the two instances in the same process.
Guillaume Ballet: Yes. Yeah, I think I think I mean when you look at the way it's implemented you know if if you start creating those instances and everything  you know the the rust code is also implemented with other lock with things like this. I think we should delete that code and just run two instances, two different programs. This is the way it's meant to be. Um I don't think we're earning we're gaining much from doing this. Um yeah, it was nice when we didn't have a st a peer-to-peer layer, but now we do. So yeah, my advice would be to delete it. And so it's okay if we have Uh but don't we need it because we have the tests the tests are being are using the ah so it's the mock uh
 
 
00:49:43
 
Gajinder Singh: Because if we if we want to delete it then we can also delete the mock network because then we don't need it and it is sort of extra code that we have to write and maintain.
Guillaume Ballet: no it's the mock chain it's not the mock network  yeah okay then yes Yeah.
Gajinder Singh: So we also have some more network test but we can remove it. I think we can replace them by e lip network test.
Guillaume Ballet: Okay, fair enough.
Gajinder Singh: Okay. I guess so once basically I think I would first let Mercy complete her integration PR with with the node command using lean quick start now so Mercy once you have done done that porting of using lean kick starter then spinning two nodes yourself then basically you know we can then in the followup here we can erase some of the mock network and beam sim although beam sim was is still pretty handy I mean to test out whether things are working fast or not but with lean quick start things have become handy as well so we can do that all right so let's just remove the mock stuff for now and if we need it again we'll read it so that basically resolves our issue of have of needing to matrix structure and passing it around because we don't want to do that
 
 
00:51:24
 
Gajinder Singh: and this way the code is also simple so we can keep following the current path that we have and and once we also remove the mock network then we don't even need to worry about maintaining the interface parity between the mock network and the sleep pub network. So that way I think we are good. But but in some of the cases maybe mock mock network might be helpful game because you don't really need to instantiate a network to test out some of the stuff. I don't know.
Guillaume Ballet: I mean, um, sure, but I mean, if we want to test out some of the stuff, usually, um, yeah, sure. Okay. A mock network is nice, but if it's getting in our way, I'd rather get rid of it. we we can always introduce a layer like if I need to test something that will be a a code module like either I test it in a live situation or it's going to be a code module so I could just mock the stuff I'm getting like I'm never going to be interested in the network itself.
 
 
00:52:37
 
Guillaume Ballet: That's my my personal case of course, but  you know, I'm just going to create some payloads that I know are going to try to cause some problems. Um so yeah, I'm not fine. As long as I can generate a chain, I don't really care about the the mock network per se.
Gajinder Singh: Understood. So I think we can take so we can take a call on this a bit later.
Guillaume Ballet: That's true. So I think
Gajinder Singh: we can right now just go ahead with the with the current matrix approach we have because it does not really break anything and we can think about the cleanup later on. It doesn't change what we are currently doing. So maybe I'll think about this and then we can take a call whether we want to remove mock stuff or not. Most likely we do but I just want to give it a rethink. Cool. Uh so I guess we have covered everything. Nup is there anything else on our agenda?
 
 
00:53:58
 
Noopur Singh: Uh, no, that was all on my agenda.
Gajinder Singh: Yeah.
Chetany Bhardwaj: I missed something. So the the build runner that is failing failing on AMD 64. So I did have a look at it and by the time I woke up today, somebody already had raised the PR. So I think the root cause that they they found out is is is pretty on point and my assumption of not having not being able to fetch the git version was the issue is probably wrong. But the the PR I've reviewed it and it's it's shouldn't I don't think it's should be merged right now because I tested it on my own as well and right now even if we have let's say like syntax error that that that PR would just make the build it wouldn't flag it and it would just pass it through. So but there are changes that we can do to fix it. Ive noted them in the PR itself. Apart from it last call we discussed the node monitoring tool.
 
 
00:55:08
 
Chetany Bhardwaj: Anshal already raised a issue for it. So I also did some research into that. So a few ideas around it. Uh well first off is if we just want a visualization tool or layer and we do not want any actions to be taken with it and we do not want it to be interactive, we could just use our current metrics. Especially since we already have lean metrics, right, which are standardized. So we can just use those to give a graphical interface and just make some grafana templates that anybody can run. We already have our metrics as an optional. So that is the most non reinventing the wheel kind of way to move forward with this. Other would be to have a hybrid approach to give a web- based or any other like even terminal UI for that matter interface where and it can be a public goods as well. So all of the lean metrics the standard ones they can be seen on one page and which could act as a global dashboard of sorts for any client and our specific metrics can again be displayed and apart from it there is something that polygon has it's more for the e side but you can have a look there is this is a good terminal UI example we can maybe use this as an inspiration and then have the UX thing going forward. So I did some research in this direction to have a better node running experience.
Gajinder Singh: understood. Thank you. Uh so we'll have a look at it and maybe you know we'll discuss it more in the coming weeks.
Chetany Bhardwaj: So, so nothing. I don't think it is urgent anyway. Just putting it
Gajinder Singh: Right, right, right. Thank you for that and I guess that will be all for today's call unless someone else has anything else to bring up. All right, guys. Thank you for today's call.
Chetany Bhardwaj: Thank you everyone.
Noopur Singh: Which one?
 
 
Transcription ended after 00:58:09

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
