# Zeam Weekly call - Meeting Notes and Transcript
# Date: October 24, 2025

## Meeting Overview
**Date:** October 24, 2025
**Recording:**  https://www.youtube.com/watch?v=9J16LyCdPqY

---
## Meeting Notes

Oct 24, 2025
Zeam call  - Transcript
00:00:00
 
Gajinder Singh: Hello everyone, welcome to October 24 Zoom call. Here we discuss our progress for DevNet one and Zim client updates uh for everyone and then we can discuss uh what is relevant uh apart outside the updates and outside any particular you know urgent concerns that are related to client development. Uh so we can start with Kai.
Kai Chen: Uh this week um uh for for team uh I still working on the uh lin spike uh PR. So got a lot of uh review uh comments. Yeah. So almost finished. Yeah. Uh it should be finished today. Uh, and I I spent uh I spent uh um some time for the Ziggly P2 Ziggly P2P uh interop test. Uh I got um so I got a um a issue uh when I test when uh test with the go client version uh 42. Yeah. Uh it it always works fine with uh with uh 40 go client 40 version but uh not not always works uh with 42. So uh sometimes the go listener uh send a close connection close frame.
 
 
00:02:03
 
Kai Chen: So I I still dig dig this. Yeah.
Gajinder Singh: And when do you think we will have something that we can integrate with?
Kai Chen: Okay. Uh I hope I hope maybe uh ne next month.
Gajinder Singh: So we will also have request response in there for just the gossip.
Kai Chen: Uh no. So we we we need a implement request response ourselves.
Gajinder Singh: All right.
Kai Chen: Um Oh yeah yeah yeah.
Gajinder Singh: So but how we will then be able to integrate it because Or is it possible that we can gossip on uh No, it won't be possible because
Kai Chen: So yeah it's is it it needs some time to to implement that. Yeah.
Gajinder Singh: then we'll have I don't know. Is it possible that we can then gossip on uh Ziggly P2P and uh request response through uh rest? I'm not sure it won't be possible because you won't be able to bind the port I guess.
Kai Chen: Uh I think I we we can wait uh wait I can implement the request and the response also.
 
 
00:03:36
 
Kai Chen: Yeah it's need some time but maybe not not much time.
Gajinder Singh: Got it. And by by the way, good job for your request response PR. Uh so it was quite a lot of work in terms of handling uh you know the sending the request and handling the responses to those requests. So good job for that. And uh yeah also just push through the PR67 that we have for the specs just to provide context. This is the PR where we are actually uh sort of segregating the signatures out from the block body. Uh so it is a step towards aggregation but that won't be happening and the follow PR to this PR would be to actually integrate uh the signature library in the spec as well as in the clients. Uh right now the signature itself is just zero bytes. uh but this is a step towards that and uh that is where we are going uh and then the next step would be aggregation where uh things would uh where basically you know you would you won't have a list of signatures right now this aggregation is into a list of signatures and then with the LMVM integration you would just have one signature and then things would so we have we are starting to get see how we are starting to get to see how uh things are going to be different and a little bit more complex to understand with respect to how we traditional traditionally think while dealing with the signatures in a block and the
 
 
00:05:21
 
Gajinder Singh: block signature itself. uh because for example because of the OTS now we have we will have just one signal signature for block and uh uh block and the proposal vote right because earlier you propose a block as a proposer and in the next interval you vote for it now you have to do that in the same uh message in the proposer itself and then uh signature is same. So, so the complications that so the complications that will come up for uh the child proposal, right? So, whoever is proposing next and wants to include the proposal vote into their block. So there are some complications for that and those are coming up uh with this particular PR and it's good that all these challenges are getting exposed so that we can reactively deal with them and uh that is something that has also been brought up to EF uh signature research team and so that basically they can make sure that such scenarios can be held by lean VM because when you are trying to pack the proposal vote So as a aggregated signature uh in the next block then basically you know it is not just the signature over uh um over attestation data it's signature over uh the entire block and vote for the proposer.
 
 
00:06:57
 
Gajinder Singh: Uh so so yeah so I think uh also this PR cleans up a lot of the other things that were missing in the spec uh because I think the last spec was bulk generated using AI and there were a lot of missing things and uh so again Kai good job uh in uh pushing that through. All right. Uh we can now go to para
Parthasarathy Ramanujam: Um thanks Kajendra. So uh I now have a fully functional zig version of the hash grade uh in the sense that uh it is deterministic within uh the zig implementation. If I use the same seed, it always generates the same set of keys. Uh and every other functionality has been tested. I have the same set of tests with comparison to rust. Uh the key generation is 1.5 times faster than rust. But the only challenge I'm having right now is uh if I use a seed phrase that I use uh in rust I don't get the same key in zig. Uh so there's some interdependency uh issue which I'm not able to narrow or rather figure out what exactly is the problem.
 
 
00:08:17
 
Parthasarathy Ramanujam: I've narrowed it down to a couple of packages. Maybe the way the inherent difference in probably rust std range uh RNG and how uh Zigg deals with it could be one of the reasons but I've been losing my hair for the past 2 days figuring out what is the issue. Uh actually I'll probably do a bit more u uh in the same thing if I'm not able to figure out I would like to raise this point. Is it really necessary given that uh both Toma and Benedict have said that uh generating keys deterministic key is not good? If uh the zig library can work in its isolation and is able to validate uh signatures from the rust implementation can we use it as it is u is my question. So uh I haven't done that part of the uh test yet which I would do so but I would like to give one more try at uh achieving complete portability between Rust and Zig. If I manage to do that then we are confident it will work as it is but otherwise yeah um so but any I'd appreciate any help.
 
 
00:09:20
 
Parthasarathy Ramanujam: I have a PR uh uh sorry an issue raised on the hashig uh repo if I have another pair of eyes looking at to see something that have gone wrong. Um yeah it would help. Yes. Can
Guillaume Ballet: Yeah. Um, yeah, I saw you you tagged me. Sorry, I'm a bit uh like uh yeah, I I have a pretty big workload at the moment. Um, the thing is I don't Yeah, I mean I don't know if there's a standard for generating uh random numbers from a from a seed. But I the way I I see or the way I understand uh it's just that you know they just generate random numbers kind of differently from uh maybe they use the same seed but they just use a different algorithm which is fine as long as both algorithms are are safe. Um what we want is to make sure that whatever the ROS code generates can be verified like if you generate if you sign something with Rust you can verify it with Z and vice versa and as long as
 
 
00:10:08
 
Parthasarathy Ramanujam: Yeah. Yep.
Guillaume Ballet: this works I would say it doesn't really matter that we don't get the same uh yeah I mean if I were you I would not bother trying to understand uh you know which one is the best uh uh I mean okay it still needs to be investigated in the sense that maybe you're using an insecure pure um version like uh to generate numbers but I've seen that the rust library sorry the zig library has some uh I mean you know the the zig library uh the cryp everything cryptoreated has been generated by this guy who created lip sodium so it's really top top of the art uh so I would not worry about something insecure being in there uh but nonetheless it might be insecure for our specific usage Uh but you know there's some actual random generator that uh like you you know you got the testing testing random seed which might actually be the same as Rust or not. Who knows? But there's also an actual um what's that called? There's also an actual um uh proper purely random generator like that goes directly to the Linux kernels generator like random number generator.
 
 
00:11:33
 
Guillaume Ballet: So if if you do have compatibility and if we know that this algorithm you use is used in the right context, I say it doesn't matter if we agree like if from the same seed we don't get the
Parthasarathy Ramanujam: Mhm.
Guillaume Ballet: same number with Rust.
Parthasarathy Ramanujam: Okay. So, as I said, I'll probably give another try.
Gajinder Singh: Yeah, just yeah just to add to this conversation when Pa you started you said it's not important to have deterministic it is important to have deterministic in the sense that you would want to regenerate the same thing again
Parthasarathy Ramanujam: Yes. Sorry. Mhm.
Gajinder Singh: and again what I I think you meant was that we are not on the same parity with the Rust library, right?
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: So in terms Yeah.
Parthasarathy Ramanujam: askition. Yeah.
Gajinder Singh: So just to correct this slight uh language over there because someone who is reading uh the notes might think that it's not deterministic in the sense that each time you generate keep pair with the same library using the same seed you won't get get the same keep pair.
 
 
00:12:29
 
Gajinder Singh: So it's deterministic in that sense but it is not on the same parity uh with the rest and as of now I think as you guys have said that it's not really a concern as long as the library
Parthasarathy Ramanujam: Yep.
Gajinder Singh: is correct which means that we can consume the signature from the rust and rust can consume signature from us so we can verify each other's signatures and that we can take care using test cases but I think in the long run it would uh still be beneficial for us to see you know what but to have a standard RNG because there have been RNG attacks based upon weak random number generators. Uh and uh the second thing would be you know it then basically for example if this goes into production and that is not a worry as of now but if this goes into production and somebody generates it with Zim library then basically onus is on to us to maintain that z library for the lifetime. uh so I mean I would not want to take that much on us as of now but uh uh so in that sense I would basically want that okay you know one tool is sort of compatible with other tool as well uh so that is there but I think that is they are not the concerns for now because we are just uh in the research stage trying to make this work and uh at some point we
 
 
00:13:39
 
Parthasarathy Ramanujam: Right.
Gajinder Singh: we will see whether you know what whether the tools uh we we want standardization for the tools or not but obviously I think RNG should be standardized and should be part of the spec so that uh basically you
Parthasarathy Ramanujam: Okay.
Gajinder Singh: know there aren't any choices left for us as a client developers to think and figure out that whether it's secure or not.
Parthasarathy Ramanujam: Sounds good.
Gajinder Singh: So apart from that, yeah, we are good for now. And uh so what what is the exact thing that made our signature generation faster than the rest? Because last time I remember you said the opposite was true.
Parthasarathy Ramanujam: Yeah. So, uh I match the parameters uh and uh the the method or the algorithm associated with tree generation u to the latest Rust implementation and when it's like for like uh everywhere rust seems to be a lot quicker at least on my machine. Um so I don't know we'd have to run a wider benchmark across other uh lifetimes as well as other machines but uh in general it turned out to be quicker.
 
 
00:14:57
 
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: Zig is quicker, right?
Parthasarathy Ramanujam: Yeah, it is. Yeah.
Gajinder Singh: Okay. Uh, another thing, do we have the latest API uh that uh uh you know Rust guys uh introduced for the tree caching?
Parthasarathy Ramanujam: Yeah, it is. It does. Uh the only difference was I had to uh self-contain the zig poseidon implementation to the uh to the hashig right now because I was continuously iterating. I'll move out my current version of the Poseidon back to the zig poseidon repo once I'm satisfied with the testing and then uh we can start to use it as a dependenc
Gajinder Singh: All right. Uh also can you give us a short uh brief on what is difference between colab 16 and 24 because the width size is different. How does that impact in terms of what we are choosing because Tomas said that uh it really isn't different. So I don't understand.
Parthasarathy Ramanujam: uh I mean uh again I I my understanding is purely from a porting perspective right the the hash generated when I using the regular one was completely different from what I used from koala by 24 and that's the reason why I did a like forl like port uh what is the underlying uh mathematical reason is not something I'm really aware of so I think that's more a question for Toma and Benedict.
 
 
00:16:26
 
Gajinder Singh: I'm assuming that colabay 24 is a bigger field than colabay 16 and I haven't I haven't really looked into it so it must be more secured by in terms of you know when the field is larger you have
Parthasarathy Ramanujam: It is there.
Gajinder Singh: more space and the collisions are going to be less so that is definitely there but my question is more with respect to what we what is the configuration that we want to use in SS Prooseidon hashing uh what is the exact field we are using right Now it is 24 or 16.
Parthasarathy Ramanujam: uh I mean uh harik uses both kola 16 and 24 uh interchangeably in few uh locations so I don't know u what should be the ideal one for ssz uh and should we I mean that's another question I was under the impression SS uses the regular SHA 256 or KACK hash right uh what was the reason behind for Poseidon uh being used there I thought it was used only for the signatures
Gajinder Singh: Shadow 56. Yes. So, uh we want to move SSH SH Shaw hashing.
 
 
00:17:34
 
Gajinder Singh: It is shaft 256 for now uh to Prooseidon hashing because uh it would be more prover friendly and faster in proving.
Parthasarathy Ramanujam: right I see Okay.
Gajinder Singh: I think it's tons and tons faster uh uh when you when you're trying to do proving. And uh we basically as as a lean spec I think there is full intention to move to uh Poseidon 2 hashing rather than rather than SH 256. Uh so so my question is basically what are the what is
Parthasarathy Ramanujam: Right.
Gajinder Singh: the length of the input that SH 256 sorry the Poseidon consumes and what is the output that Poseidon gives because in SH 256 you take 32 you take basically 32 bytes of chunk right so you digest them and you get again 32 bytes output right so uh what what is what is that uh input and output width over here because from what I remember they are saying that uh the
Parthasarathy Ramanujam: I I have to come back to you on that. I don't remember at the top of my head. Let me look at the code and ping you back.
 
 
00:18:41
 
Gajinder Singh: Merkel root of uh sigash the public key is 52 bytes right so if it's 50
Parthasarathy Ramanujam: 52 bytes. Yeah, but that contains uh Yeah, but that contains multiple parameters. One is the root element. The other is the list of parameters which are encoded into the public key. So, uh the sha uh I mean uh the pose to hash there is a combination of these two. Um, so I'm not sure if uh you would get a deterministic size of the hash with pose to it depends on what you're actually hashing.
Gajinder Singh: I mean, but what what do you mean deterministic size? Because there has to be a max size that we will basically pad to, right?
Parthasarathy Ramanujam: So that's that's true. But when you're hashing, you only hash the root, right? The public key here uh uh in the hashig uh represents uh essentially contains two component which is the root hash which is uh a hash of the uh uh the entire Merkel tree plus certain things and the other they also uh uh what do you say include the parameters which was used for the key generation and encode it into the public key and this entire uh JSON strct is what I believe is serialized uh to 52 bytes if I'm not mistaken size.
 
 
00:20:00
 
Gajinder Singh: Got it. So what so can you figure out what is the exact uh root size that is there because we don't we don't need to embed other parameters when we when we are doing SSD hashing.
Parthasarathy Ramanujam: Yeah, I I'll come back to you on that. I need to uh I haven't actually measured it, but I'll I'll come back to you on that. Yeah.
Gajinder Singh: So yeah that is there. All right. Uh thanks Partha. Let's move to Chatanya.
Chetany Bhardwaj: Uh hello everyone. So this week uh I'll continue from where Tata left. Uh uh I did a basic implementation integration for Poseidon and a couple of things that I could add on top from what I know again take it with a bit of grain of salt. I did mostly it in terms of integration. So rather than having a sha like bite uh constraint a poseidon is more constrained on the field uh size. So the the the integration that I did is is primarily for field size of 16 that embeds well uh with the SHA like APIs that we created and as our existing SSC expects uh chunks of 32 bytes.
 
 
00:21:17
 
Chetany Bhardwaj: So that corresponds well to that as well. So uh there is that and that that is out right now as well. again uh I I am I put out uh the PR in draft mode if anybody wants to have a look. uh and I was uh testing it and there are some uh changes that Partha has made uh for the rust compatibility which also changes a lot of things on our end but uh uh I I think those are mostly for the tree size the the rust compatibility changes that they do not correlate to uh our SSC since we use our own merization over there for tree generation. So uh but uh there there needs to be some more changes to point to the latest uh changes that Partha made in Zig Poseidon. I'm testing it on my own. Uh I just for the SSD side since there are no other libraries uh that have Poseidon hasher over there. Uh so that is something that is going on. Uh and I'll I'll get that out soon as well.
 
 
00:22:21
 
Chetany Bhardwaj: Other thing that I picked up on was uh the lean uh quick start. I was just trying things out on my end to see how it goes and I I found a I was just working on the Mac OS and more compatibility fixes over there. Found a few small issues u mostly unrelated to our stuff but the one that was uh bugging me for Zim was the one that I shared in the chat as well. Uh the image that we have on the docker hub is probably broken and uh uh it's uh again it just crashes right after starting. So I couldn't get it to work. I built my own local image uh which is not AMD 64. So uh but that one worked and I I put out a draft PR for that as well. I'm just waiting for adding the metrics thing to the quick start as well. just waiting on uh the lean metrics things to be done uh by Karta. If if not we'll have our own interface as well. I have done some minor work over there.
 
 
00:23:26
 
Chetany Bhardwaj: Apart from it, I had another question. Uh when do we plan to move to uh Ziggv0.15.1? Is it post DevNet one or what is the timeline? Because there are some issues on SS side that GM has raised. uh and uh I have done some minor porting uh but the one that uh the issues that K mentioned moving back to a stack based array and using pointers those changes are left. So if I could get a clarity on that I could decide on what to take up on priority.
Guillaume Ballet: I mean it's just my opinion of course uh you can decide to disagree like you being everybody in that room uh can disagree uh but my my guess would be to wait for 115.2 two before we attempt to to move to to the 15 line uh simply because it was such a big change that people libraries everybody still struggling um and I think uh you know 1141 works pretty well so if you run into a major problem that's a different question but uh as long as we don't have any showstopper uh we should totally stay stick to what we have uh because you know like if we move to 115.2 there would be 150.3 uh and so on.
 
 
00:24:52
 
Guillaume Ballet: So we should really uh wait for more stability and I think 150.1 at least okay I didn't check recently but uh two two three weeks ago it was still a lot of drama a lot of uh people struggling to to keep the compatibility so I think uh the best way you know we we're not obliged to stay with the latest because even if there's a security flaw at the moment we're still doing very much uh uh very much prototyping work. So it doesn't matter if there's you know some kind of vulnerability anywhere in the li in the libs. So in my view there's no rush to upgrade unless you do find a critical flaw that forces us to do this. But uh I think for DevNet one we should still stick to the same version of the compiler.
Chetany Bhardwaj: All right. All right. That makes sense. Oh, but just because of the issues, I was under the impression this is something uh that we want to get done soon. Maybe I misread it.
Guillaume Ballet: Uh okay.
 
 
00:25:58
 
Guillaume Ballet: Like you mean the issue inside the like which issue? The issue inside our issue. Uh ah sorry that one.
Chetany Bhardwaj: uh the SSG the issues that you created for the upgrade from bounded arrays.
Guillaume Ballet: Yeah.
Chetany Bhardwaj: Yeah, because those APIs change as well.
Guillaume Ballet: No no that's just uh that's just a reminder. But honestly I would uh I would stick to Yeah. No, no rush.
Chetany Bhardwaj: Okay, got it. Got it. Thank you.
Gajinder Singh: Yeah, for other things uh uh so one of the thing that you mentioned was that you have used field 16 and uh we create chunks of 32 in our SSD library. So what are you doing? You are petting them with the zeros.
Chetany Bhardwaj: Yes. So there is zero padding for smaller inputs.
Gajinder Singh: Yes.
Chetany Bhardwaj: Uh for larger inputs right now it's uh it's ideally not even going to work because it's just going to cut the the inputs off otherwise we do zero padding. This also introduces some level of collision that I have mentioned in the PR itself.
 
 
00:26:59
 
Chetany Bhardwaj: But given that we all for our SSE purposes it should not affect us because we are always giving uh the chunks of same 32 bytes. So but again it's not a general purpose solution that other should take inspiration from especially when working on signature stuff. It's just a SSG uh wrapper just specifically for that.
Gajinder Singh: uh just so to be clear basically field 16 and field 24 I think these already represent the size so they sort of resolve my confusion of what is the output size of the hash that's going to be because it has to be represented in these field bytes it.
Parthasarathy Ramanujam: It's 32 bytes uh
Gajinder Singh: Okay. I mean maybe they are padding it internally because if the field is 20 24 bytes size then it should fit ideally in 24 bytes but yeah I mean I'll basically check into details of that because I'm not a cryptographer in that sense. Uh but okay so we have 32 but in Kabay 16 what is the output? It is still 32 bytes.
Chetany Bhardwaj: Uh so right now uh I'm using the compress function that is provided in zigosidin which gives a configurable output internally again not sure how it uh does the handling for it but it does give out 32.
 
 
00:28:29
 
Gajinder Singh: Okay. So, so right now for uh for the purpose of SSD integration, you're still able to get 32 bytes output.
Chetany Bhardwaj: Yes. Yes. All the integration has been done in just to make sure it works with the existing APIs that we have.
Gajinder Singh: All right, cool. So I will look into the PR and obviously uh Gim will also give a look. U all right so thanks Chan and let's move to Nur. Yeah.
Chetany Bhardwaj: Yeah, again it's more of a work in progress right now. So feel free to uh suggest things over there. It's again I did a lot of things just to my understanding on how I could get it. So again, not I I don't think this is the final version that will uh merge, but yes.
Gajinder Singh: Yep. Sounds good.
Noopur Singh: Hi. So, uh I have uh created uh a PR for uh writing finaliz and and finalized uh slot to block root index to the database and uh uh like before that that's a PI I created and before like moving on to a new task I was just doing I I was picking up some minor housekeeping tasks and yeah that's it.
 
 
00:29:56
 
Gajinder Singh: Thanks Nupur Mercy.
Mercy Boma: Good afternoon. So um during this week I did a lot of minor minor attacks. one has to do with um auto um updating the number of validators in config.ml while on the gen on the lintspec on quick start um repo and then I did also a I added a validation check for choice statistication process on the specs repo but currently I'm looking at the test vectors and how we're going to implement it on our own side I'm still trying to properly understand um the PR that Filipe I don't know if I got his name correctly put out and also how we're going to implement it but I have um um what's it called I did notice um when I I tried running um a test something failed I'm not sure why but if I confirm it I will raise up the questions and then get clarification on
Guillaume Ballet: So if you fail the test, there is a PR that I created that uh you can take over if you want uh to consume those tests. It hasn't been tested because the tests were not filling at the time.
 
 
00:31:14
 
Guillaume Ballet: Uh but we yeah you you can take that over and try to to find whatever has been filled which should be normally in a dict in a directory called fixtures. And if you can consume this inside the inside my program, which will probably require a lot of changes, um that should be able to Yeah. Okay. It's probably not going to work right off the bat, but it's uh it should get you started.
Mercy Boma: Okay, thank you.
Gajinder Singh: Yeah, just to add to that I think uh there is this PR 76 which I just shared in the ZIM community channel uh which I guess we are talking about and uh I am I will be reviewing this spec uh test PR and uh basically we'll discuss it on Monday with other teams uh of how we are liking it, whether the format is all good and uh we are good with uh the way it proposes to consume the spec test. So everyone else is also welcome to look at it because I think uh spec test is going to be next week's priority if basically you know we are good with what has been proposed.
 
 
00:32:39
 
Gajinder Singh: All right. Uh we can now move to Unshell
Anshal Shukla: Yeah. So, uh, initially like there were some minor comments that Kim had put into hack 6P that I had raised and mostly like I have addressed uh those comments apart from like removing the removing the feature flag because if I do that for that I'll need to like remove the other tests that I have added too. So I had put it up on the in one of the comments in the PR itself that do we want should I like just remove all the other tests and remove the feature flag as well or uh or like should I wait for game to like merge his uh splitting up of Rust uh code um that's one thing uh apart from that I was working on uh working on fixing the uh fixing the XCV scheduleuler so there right now like there was some issue with uh scheduling of uh events happening crosschain. So I fixed that up. Kai has already reviewed it. He has put up a couple of uh comments there.
 
 
00:33:54
 
Anshal Shukla: Uh I have like already addressed one of them locally. There's another one uh which I'll do after this call and I should be like uh I should be able to push it by tonight. Uh apart from that there's another minor PR uh mostly on the housekeeping front. Uh so after like the request response PR got more there were like a bunch of things around parent block syncing. I have commented uh I have tagged you on I think two three PRs where I which I think has already been addressed and we can close them. Uh and I have raised uh uh raised a PR for uh one of these issues. Um, uh, yeah, that's it for mine.
Gajinder Singh: All right.
Guillaume Ballet: Uh yeah, regarding the other PR you were talking about, uh I saw your comment.
Gajinder Singh: Uh, I'll take a look into it.
Guillaume Ballet: Uh if you can give me I'm currently I guess that's my update at the same time. If you can give me a couple more hours to merge the the PR to yeah to uh to split all the libs and then we can rework it.
 
 
00:35:06
 
Guillaume Ballet: Uh and if it makes your life simpler, I think we should go this way. uh if I can't manage to get it uh through because it's been very complicated, surprisingly complicated, um I will uh just say just merge it and we'll uh you know I'll start over and try to to break it this way. But I think it's worth simplifying the build system which is currently very very complicated. Uh so if you could give me two hours and then we can uh we can better answer that question.
Anshal Shukla: Yeah, yeah, sure. I I I don't think like we need that pair right away because before doing that, we have to make the changes for the PR67 as well and then we'll be like integrating those uh signatures and like uh PR67 is yet to be confirmed on lean spec then we'll have to port that on Zim as well.
Gajinder Singh: Yeah, PR67 is almost cons confirmed and I think maybe you can take it because uh guide us back. So it's better that uh other person in our the some other person in the team uh implements it in the Zam so that the knowledge is spread out.
 
 
00:36:15
 
Gajinder Singh: Uh so in that sense I guess you can pick PR67 to implement on theme because we are almost there and we might actually end up merging it today.
Anshal Shukla: Yeah, sure. I'll do that.
Gajinder Singh: Just a little bit of name changes I think mostly. Okay, with uh my update is that I have uh been working on PR67 with Kai. Uh have been looking at other reviewing and merging other PRs uh and uh was digging a bit deeper into Kai's uh request response PR because that was a that was a big change that we merged in and now using that we need to also implement uh uh parent chain syncing. I think uh this is something uh u it could be a little bit more challenging because we also have to be optimal in that and uh so this task is up to be picked up and whoseever wants to pick it up can claim it. with regard to uh our progress for DevNet run. Uh I think now now we have this and then in our uh lean quick start uh there is Mercy's PR that I need to review to uh which basically also adds uh uh which also generates uh uh keep pair to be enrolled uh by the teams into their genesis trait.
 
 
00:37:59
 
Gajinder Singh: Uh so once we have PR67 merge then I'll basically also uh look at uh Mercy's leans uh lean quick start PR and see if it correctly generates and then we can merge it uh and also basically take a look at uh what are the timings to generate small amount of uh uh to generate small amount of validators for small lifetimes uh And I think with regard to spec right so next thing that the onus is on on our team to also add uh add signatures in the spec as well. So uh that task is also up for grabs uh whoseever wants to do it. I'm not sure whether uh whether hashtag is available in python. I don't think it is available but is is it available with bindings in python uh because we'll need to integrate it and it's a working spec so somebody would need to sort of generate python bindings for it I think because we are not going to implement hik library in python itself. Uh so that is something that we need to figure out.
 
 
00:39:20
 
Gajinder Singh: Uh para can you take a look at it because I think this is sort of closely related with the work that we're doing.
Parthasarathy Ramanujam: Sure. So you need Python bindings for the existing hash secret if I understand correctly. Is that correct?
Gajinder Singh: Yeah, because I think uh we will be using rust uh library in the spec itself. So uh that we need bindings how to basically you know integrate uh uh call that uh library from the rest and integrate into leanspec.
Parthasarathy Ramanujam: Okay. Sure.
Gajinder Singh: So I think if you can do do a PR for that in leanspec that would be awesome.
Parthasarathy Ramanujam: All right. Okay, I'll take it. Thank you.
Gajinder Singh: Okay. Uh so basically if this happens then it will sort of cover up the spec for DevNet one and uh I think we should be able to implement it in a couple of weeks. So that solves our DevNet uh one target. Uh and then we have Poseidon hasher which I think I will try to push for the next devet.
 
 
00:40:36
 
Gajinder Singh: Uh so with regard to that chan can you also look uh what are the what what is what are the poseidon hasher specs on leanspec repo so that we are on top of that.
Chetany Bhardwaj: Sure, sure, sure. So, right now just looking at those and making sure we are uh in alignment with that.
Gajinder Singh: Got it. Cool. Anything else that comes to your mind, guys, that we need to focus on? Yep.
Chetany Bhardwaj: I I I saw there was this uh new task about an uh execution client uh integration get integration. So would that be split up or or what's uh I'm just generally curious about that if we could go over that one.
Noopur Singh: uh that is actually uh like uh for the EF uh uh internship what Zim could offer. So I have created that as a part of that that is not uh a part of uh like this project right now.
Gajinder Singh: Yeah. So basically in PQ Devnets uh we are not going for uh execution integration as of now because the focus is towards uh postquantum cryptography and uh P2P.
 
 
00:42:07
 
Gajinder Singh: Uh so yes that is a parallel project that we might offer for EPF.
Chetany Bhardwaj: Thank
Gajinder Singh: Cool. Uh so Jose Jose right uh would you like to add anything because you are here for the first time so welcome and would you like to share anything with
José Hugo De la cruz: Yeah, thank you. Um, no, from my side, I've just been looking to the lean road map and yeah, I was interested in looking into this project and see how it works and maybe learn a bit about it.
Gajinder Singh: Got it. Cool. Thank you uh for being here and uh hope to see her in future as well. Let us over to you. Any news for
Ladislaus: Yeah. Hi guys. Um well news I mean um worthwhile listening to what what Justin mentioned on our Wednesday Wednesday call. I think he gave a uh an interesting like more holistic like project up upgrade. Um I think I personally am still digesting the the Cambridge workshops a little bit. um obviously the the postquantum and and sort of the lean lean more lean specific uh outcomes.
 
 
00:43:35
 
Ladislaus: Um really hope that you guys will be able to to take a look at the recordings and the and the summaries over the next few days. Um yeah, the British food well Cambridge food is actually in it's quite uh co uh cosmopolitan I would say. So it was good. Um well but we I guess what I would also hint at is that we we had um we had a consensus there an architecture workshop following the postquantum workshop. Um and so there obviously um we we we discussed um we discussed faster finality mechanisms again. So maybe this may also have an impact on on the lean road map. Um it's still a bit early to tell but um but yeah exciting progress which researchers have made over the following few days and um yeah what else? I mean yeah the one year anniversary coming up. I think it's um for this group and and for all client teams working on lean, it's um yeah, it's um it's pretty fascinating to to look at uh the past year and and I guess for us as as as people trying to steer this forward.
 
 
00:44:47
 
Ladislaus: It'll be interesting to see uh what we make out of uh out of the anniversary in terms of like what what's next? um how do um I guess what I'm thinking about is how do we integrate um more client teams into lean consensus right as you realize we have we have uh interest u from from additional teams nowadays um and then also the existing time teams um so yeah I guess it's I'm a bit more of a lean diplomat these days than than I guess having having a lot of time to uh to dive into into the very technical details but I guess overall very exciting progress on a lot of fronts. Um it'll be like lean is here like after the workshops and after the anniversary lean is here to stay and now it's a bit of a question for us all to be also bit mindful how do we merge like gracefully merge these ideas into into the broader Ethereum road map and um yeah so and pushing hard on one end and sort of implementing and and and interoping is is one thing but also like being yeah some somewhat mindful about um how this fits into Ethereum eventually and and it looks like we have a we at the point where um where we are yeah we are where this is going to be a a question and a task for us.
 
 
00:46:14
 
Ladislaus: Yeah. So bit of a rant but this is where where I think about a where yeah what I think about a lot these days where I talk to a lot of people about these days and and yeah I hope to um yeah but but still hope to continue sort of following the the discussions the more technical discussions which are happening.
Gajinder Singh: Thanks Larus uh for sharing and yes we uh are waiting for all the videos to come out uh and to see all the exciting discussions and talks that happen in Cambridge. Uh I hope there aren't any videos of British food I guess from what I'm hearing but yeah so uh with regard to uh the relevance of the work that we are doing and how it adds value back to Ethereum I think yes that is a very p pertinent question and uh it is a question that we should be asking ourselves as well as discussing with with others and in that sense I think uh we we focused on PQ you because that was definitely one clear thing that we can uh uh contribute back uh to Ethereum uh mainet uh so that we are de-risking the entire uh postquantum strategy uh and basically figuring out all the challenges and questions that need to be asked much before we come to a point where quantum becomes a threat given we had some uh recent news about uh Google's Willow project doing some interesting stuff.
 
 
00:47:55
 
Gajinder Singh: Uh so I guess I think uh the quantum uh threat could be real could not be real. I'm not sure because it is very hard to predict uh uh what are the what are the progress that quantum computers are going to make over coming years. But uh definitely I think our work drisk drisk drisk that uh strategy and make sure that ethereum as a whole is ready for it and going forward for forward from the postquantum definitely. Now the question comes that what are the other areas that we should be exploring to contribute back to Ethereum and one of them is definitely uh consensus. Right now we are using 3SF mini but uh uh we need to focus on uh uh on more current uh uh finalization approaches and I hope I think that is what we should work on and figure out uh post uh we have completed our quantum work. uh rainbow staking also I think is very important because if uh proving is going to become an integral part of Ethereum ecosystem then we will have to shift to a place where uh there are different roles for different nodes uh because or not all nodes will be equal and uh this is where rainforking will come in and uh as lean consensus I think the onus is on to us to figure out how it could look like
 
 
00:49:28
 
Gajinder Singh: And again to present uh these options to Ethereum ecosystem to say that okay you know this is how it could look like and these are the parameters these are the uh tradeoffs. So uh you know help them choose with the metrics that Ethereum ecosystem could be comfortable in. But yes, all all these are all important questions that we need to ask ourselves and we basically need to have an active engagement u with those doing mainline Ethereum development because we can't uh develop in silo and we have to basically make sure that whatever work we are doing is directly uh usable by the uh Ethereum mainet community.
Ladislaus: Yeah, 100% agree and and I think the this boil or this sort of crystallizes in this sense also in in decisions we're making uh very technical decisions or uh for example if you think of the the test test vector uh pull request right it's is we as much as it's it's it's great and uh to start from a clean sheet and sort of learn from mistakes we did we uh we obviously deviate from from mainline Ethereum and mainline consensus processes and and as much as we should embrace like making progress and sort of um doing it quote unquote better um we yeah uh we still yeah I don't know we at least need to be mindful or or like have have um fallback mechanisms or or at least ideas and thought about how how would it be possible to to um like if if there's not a big lean fog quote unquote like how to
 
 
00:51:19
 
Ladislaus: incrementally um ship these features and and I guess it's it's also like I don't have a technical answer for this. Uh I can just raise the question I guess like can we think of ways to to which which I referred to earlier as this this um graceful migration if you will. How can we still be able to migrate uh to to incrementally ship um lean lean progress in into existing um uh cons consensus layer developments. Um so yeah, it's a powerful question, meta question. It's it's less of an answer, but but I think yeah, you summarized it well. Um it's important to not deviate too much in the sense that we uh yeah we just are the second uh sort of uh work stream which which which loses attachment to to Ethereum itself. Um, so yeah, be mindful about it and and and think about like ways to um to to stay to stay compatible, I guess, with um with Ethereum C existing Ethereum CL.
Gajinder Singh: Yep. And we are also having some influx of uh Ethereum CL devs looking starting to look into lean spec.
 
 
00:52:35
 
Gajinder Singh: So there is also some crosspollination already some crosspollination happening and some feedback coming from there as well. Uh so we should basically accelerate and encourage more of this so that uh ideas that we are uh working on over here are not totally foreign to them and uh basically you know they can take back the learnings from what we are doing over here. All right. Uh so this brings us to the end of today's call and we are almost time. Y say yeah.
Mercy Boma: Sorry, I wanted to ask is there like any other um pending tax in respect to devet one that is needs an attention to
Gajinder Singh: So I think uh the pending task would be then next to integrate uh the signature library into ZIM uh because that is once we have PR67 implemented in ZIM then the next follow PR is to actually integrate the signature. So that would be there and I'll take a look at other pending things and uh see how to push forward uh how to make our client more robust and better. So I'll take a look and uh come back on this. All right. Uh thank you guys for today's call.
Chetany Bhardwaj: This
 
 
Transcription ended after 00:54:37

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
