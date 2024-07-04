# Facial Workflow for ELEX 2 

Coming from Elex, where I had very little time to build all our faces and get them articulating, with ELEX 2 I wanted to make things right.

One major downside in the first game was the fact that I needed to use blendshapes and had to keep the shape count low for performance reasons. Also, the majority of faces Ire delivered very late, so I had to just apply the deltas of our Player head to all the different heads, which naturally looked sub optimal.

## Build a joint based Face

To solve the performance issues and make our lives easier with the transfer of the rigs to other faces I set out to build a joint based rig! The plan was to use this rig in Engine. Unfortunately the file servers for the project were shut down after the studio closed down, so I do not have a working live rig with controls to show. 
But don't fear, I'll show what the skeletal structure looked like in a moment. 

So when I demonstrated the rig, everyone was pretty happy. But when I started to talk to the devs of our in house engine, it turned out that the task of storing a specific initial joint prosition for each characters face rig was nothing that could be implemented into the Engine in a timely manner, since all of our coders were busy with other important tasks. It seemed like I had to fall back to blends only, again! (this includes eye and jaw rotation).

So using our actual rig I created a pose file that contained the Head, driven by joints, moving throigh all poses I thought I might need for a complex blendshape based rig. Here is a quick clip showing the joint driven head going through all poses:



## Convert it to blends

The workflow that I created was fairly straightforward: I created a dictionary of shapes I wanted to use for the rig, and the corresponding frames on our pose file. Then I wrote a script to process the head! Here is basically what the script did:

 - collect all parts of the head (head, brows, eyes, eyelashes, teeth, tongue, cornea...) duplicate and combine them into one single mesh. This will receive our blends.
 - since all of our parts are seperate and following different skin clusters, I opted to go through each pose, combine the meshes again in the given pose and add them to the blendshape list
 - some blends were symmentrical although I wanted to split the into left/right ode up/down shapes (or even more for the sticky lips). I wrote a script that saved and loaded predefined blendshape weights and allowed to split the shapes even more.
 - In the end, with all blends created, I geberated a clean head, receiving all blends. A buffer node was created with a simple float attribute for each blend.
 - Our simple Control Rig then connected our inputs to the buffer node - that way I could load any head into a scene and connect it to the control rig.
 - Most of the complex rig logic, like how the blends are combined etc. is driven by the control rig
 - in the End a Skin Cluster is loaded on the final Head in order to make it move with the body rig!

Here is a quick capture of the Rig generation:



## Other Heads

With the Main head out of the way, I had to tackle the remaining 118 heads (which, as usual were delivered VERY late in production). I wrote a simple tool that would map all of our given joints to the new heads proportions using the topology (since I were using a unified topology for all heads). I made some basic proportions for basic proportion differences on the joint movement, but not to the details I would have liked - there simply was no time!
One other aspect was, that the topology on some heads was all over the place, with lip loops voving far out of the Lip area or inside the mouth! I wrote a quick tool that would slide over a copy of the head surface and drag marked loops along, in order or do a minimal quick fix (not shown in video, as I don't have unfixed head at my disposal anymore - sorry).

Building the Rig on a new Head:



## Lip Sync / Idles

I believe I had around 80.000 voice files (.WAV's) in 4 Languages to process. As was tradition, the Voice recordings were unfortunately delivered very late. I used [Speech Graphic SGX 3](https://www.speech-graphics.com/sgx-production-audio-to-face-animation-software) for handling ALL of our Lips and Face animation. This tool was a lifesaver!
Heads need to be solved into a template file at Speech Graphics. I could only get one Male and one Female rig get solved, so all faces in the game use one or the other. In maya I created a repository of Hi and Low poses in different moods for both the male and female template file in SGX's proprietary maya tool. 
(Unfortunately I do not have a working lic anymore but here is a [demo Video from SGX](https://vimeo.com/841922761/48258cb5de) in case anyone is interested)

Any a quick demo of a few lines being applied to several different Faces:

[![](http://img.youtube.com/vi/_JWZDS5ZSC8/0.jpg)](http://www.youtube.com/watch?v=_JWZDS5ZSC8 "A short video showing 3 Heads that are playing an automatically generated animation, created in SGX based a wavw file and a text transcript")



## Conclusion
Given the time and Manpower (me :) ) I think the results are not bad. There is a lot that I would reconsider, and I already had some plans in place for the next title.. but as of yet, it seems like this won't happen!

Things I would improve for sure:
- Hybrid joint based rig with few Blendshape Correctives
- Better Wrinkle map integration (in Engine)
- Better algorythm to transfer joint animation between different faces
- More streamlined rig generation 
- add Machine Learning to the transfer system
