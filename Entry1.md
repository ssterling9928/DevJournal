# Task Prompt 

> 🚀 Day 1 Task: “Show Your Code, Even the Small Stuff”
> 
> Objective: Begin building your visible footprint as a developer.
> 
> Task:
> Create a new public GitHub repository called “daily-dev-journal”.
> In it, add a simple README.md file describing:
> 
> What kind of developer you want to become (e.g., “C++ game dev focusing on Unreal Engine projects”).
> 
> What you’re currently learning.
> 
> Your goal for the next 3 months.
> 
> Then, commit and push the file.
> 
> Why this matters:
> Recruiters and hiring managers love seeing consistency. Even small repositories show discipline, curiosity, and growth — all signs of a developer worth hiring.
> 
> Bonus (optional):
> Add one small snippet or function you wrote recently — even if it’s tiny. A working HelloWorld.cpp is infinitely better than an empty repo.

---

My main goal is to become a game developer.  I enjoy using both Unreal Engine and Unity Engine for game development, so landing a role in either or would be a major accomplishment for me. 
When it comes to learning, Udemy is my main go-to.  I really enjoy the online learning process, and I love the way that Udemy does this.  However, in the current moment, I would say the newest thing I have taught myself is *Notion*.  In my own words, Notion is all around project manager.  You can use it for collaborating with a team, managing and tracking data and schedule timelines, keeping up with project tasks, and much more.  Recently I have started my own little Project HQ database for personal tracking.  My goal for the next three months is to start my career in development.  Even if I am not a "game" developer, I would still love to have a role as developer of any type of application.

Just a quick excerpt from my CircleJump game that is ongoing.  This portion of my code is what transforms the object from world space to the controlled cylinder space.  These properties take in the current position, and calculate the placement on the cylinder grid.  This is still a work in process and subject to change, but this is current as it stands. 


```csharp
    public Vector3 WorldPosition
    {
        get
        {
            float x = Mathf.Cos(theta) * _radius;
            float z = Mathf.Sin(theta) * _radius;
            Vector3 localPosition = new Vector3(x, y, z);
            return meshTransform.localPosition + meshTransform.rotation * localPosition;
        }
    }
    public Quaternion WorldRotation
    {
        get
        {
            float x = Mathf.Cos(theta);
            float z = Mathf.Sin(theta);
            Quaternion outwards = Quaternion.LookRotation(new Vector3(x, 0, z), Vector3.up);
            return meshTransform.rotation * outwards;
        }
    }
```




