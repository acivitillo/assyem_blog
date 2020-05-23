My Ideal Python for Data Analytics Workshop
2020-1-29
Process

Recently I trained few people on python for data analytics. This was on the job training and we had to get things moving quickly. I noticed few things that are core to accelerated learning.

* Have a real life project that you want to accomplish. Better if it's a project that can be published to an audience
* SQL is the first real step into analytics, always
* Work with other peers. If you are not part of a peers group find one
* Get coaching from someone who has done it *in production*
* Learn how to find the answers you need to keep going or to ask for help on online forums
* Have a formal way of communicating within the team. This is key. Lack of communication kills both teams and projects

Noticed I haven't said **course** or **theory** or **books**. All of those things are still essential but they won't be predictors of success, at least not accelerated success. The biggest predictor of success is learning python because we want to build something we care about.

Now what to do if we want to put together a workshop to accelerate learning of python for data analytics?

## Outline of a Workshop

Now that we got that out of the way, here is an idea for a short workshop of 6 hours.

| Hours  | Subject                                | What                                                                                                                                                                    | Why                                                                                                                                                                             |
|--------|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 0-1    | Pick the projects and the teams        | Let people come with their own project ideas to the workshop, then let them pick projects to work on and teams to work with                                             | Build a peer group. Let them find a project they are excited about.                                                                                                             |
| 1-2    | Install all that is needed on all PC's | Set up the python environment in each machine correctly. Install git, anaconda, jupyter, etc.                                                                           | Although we will require them to do this before the workshop, we still  expect problems and allow for time to make sure everybody is setup  for success                         |
| 2-3    | SQL and Python basics                  | Use Python to fetch the data and upload into SQlite. Use SQL to do some first basic processing of the data in SQLite. Intro to Python Pandas                            |                                                                                                                                                                                 |
| 3-3:30 | Communication **within** the team      | Explain how to use GitHub to effectively communicate in the team.  Create issues and milestones for the project. Create proper  README.md and doc files for the project | The biggest project failures tend to be  communication failures. Although GitHub  may seem obvious at a first look, people  that never used it still find it hard to  get going |
| 3:30-6 | Do the work                            | Workshop coaches will facilitate the work during this time.  Special emphasis here on finding the answers online vs directly  giving them the answers                   |                                                                                                                                                                                 |

## Variations

Many variations of the above are possible but the key is making sure the participant apply python to something real. If that's happening, pretty much everything else can be modified accordingly.

## Automation

On a more technical side, in analytics everything is a workflow. We can look at it as a set of operations and activities that we apply on data to get insights and recommendations. Normally people that do this do it in Excel which means they can't automate it that easily.

It's important to emphasize automation from the get go with Python because 1) people hate manual repetitive work but 2) it forces many good programming and thinking practices. Automation is also **key** to quality insights and reduction of manual errors.

I recommend to use dask [Dask](https://dask.org/) for teaching Python **and** automation at the same time.

## Communication

It's hard to predict how a team will perform. In this case we have 2 types of teams. We have the project team that could go from 1 person to 2-3. Then we have the wider workshop team that could be 15-20 people or more. So we have 2 layers, I recommend to set clear communication rules for both.

The easiest thing to do is to use the github issue tracker and label issues as questions, tasks or issues. Then apply exactly the same setup for project teams and for the entire workshop. This way all communication will flow easily and facilitators or participant will be able to link back to answers to solved problems.

## Conclusion

There are many courses and books out there on Python. Many of them are great. But a lot of them focus on Python in isolation, without looking at the broader picture. People that want to learn Python 1) want to apply it to some real life problem; 2) will need help from others; 3) will work in teams; 4) will need to communicate their results to management. So learning just Python is not going to be enough to be a successful data analyst.

Here I tried to put together a workshop outline that gives participants an intro to working in real life on these projects. It will not be a workshop for people that want a deep introduction in machine learning or data science, but it will serve very well those individuals that want to get started to working in data analytics with Python in production.
