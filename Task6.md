
# New Journal Entry - April 27 2026

I know its been a while since I have posted in my dev journal, so I wanted to add an entry.  I have been working on multiple other projects.  One project I am apart of at the momement, is a dealership website that will include all of its inventory.  I have completed my part for as far as I can at the moment, and we are waiting on some internal work to be done.  I have posted a little excerpt from that below, as I have the repo private at the moment. 

<img width="606" height="723" alt="image" src="https://github.com/user-attachments/assets/bc91de3c-a04f-4d49-bca2-e3dbbda81bf1" />

---

Another Project that I have been working on is my Zombies game in Unreal Engine.  I just updated my editor verision to the newest 5.7.4 and trying to get back into it.  I left off working on zombie animation during attacking.  I am happy to say, that I have that completed and moving onto working on the character now.  This repo is public so be sure to check it out.  

[Check out Zombies!!](https://github.com/ssterling9928/Zombies)

--- 
One issue I had with my Zombies project was trying to figure out why my zombie was not attacking.  I had ChasePlayer() already set up, and that was working, however when reaching the character it would just stop and do nothing.  I struggled a bit to find my error.  After some debugging and some research, I found that I did not include the "slot" in the animation output, so the attack montage had nowhere to play at.  Once I added the slot to the animation blueprint, it worked like a dream.  

Wish me luck on my continued journey!! 

