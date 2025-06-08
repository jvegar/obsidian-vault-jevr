## A Vim-Inspired Approach to Efficient Note Management with Obsidian and Markdown 
I'm currently on vacation, and it is time to dive into one of my favorite topics: **knowledge workflow management**. As I'm sharing most of [my notes](https://brain.ssp.sh/) and even [my book](https://dedp.online/?ref=ssp.sh) publicly, it might be interesting to see my knowledge management workflow. I'm also journaling, reflecting, and connecting all my notes, sparking most of my insights into my sharing. All of it happens in plain text in my note-taking app. This article will detail my [Obsidian](http://ssp.sh/brain/obsidian) workflow, which many of you have requested. That's why I'm sharing some more details here.

As you might guess, I have a very dedicated workflow. Sometimes, I even get jokes about how organized or methodical I am. I'm not shy about spreading the word about why you should use a second brain and store all information in a central place.

But once at a time. Besides my deep dives, I wrote about [Personal Knowledge Management Workflow for a Deeper Life](https://www.ssp.sh/blog/pkm-workflow-for-a-deeper-life/), [My Vim-verse](https://www.ssp.sh/blog/my-vimverse/), or [Why Vim Is More Than Just An Editor](http://ssp.sh/blog/why-using-neovim-data-engineer-and-writer-2023/); this article focuses more on the Obsidian and my workflow and how it ultimately led me to more clarity and genuine insights. Key Takeaways are why I use Obsidian for note-taking, the role of Markdown in my note management and essential plugins I use.

#### Check out the YouTube Video
>Update: I added a [YouTube Video](https://youtu.be/myHKHM2mIis?ref=ssp.sh) to showcase my Obsidian workflow visually. If you prefer watching over reading, check it out below. You can also check out the shorter five-minute version of [Vim with Obsidian (No Mouse)](https://youtu.be/LQasaw4MkqE?si=UKRpxwnzGKFHVPlN&ref=ssp.sh)

#### It's Not about the Tool
>Obsidian is the tool I use, and I will share a bit more about it. It's not about which tool you use, as you can achieve the same with any other.

# My Workflow For a Deeper Life
Everything in my workflow and note-taking approach is [Plaintext Files](http://ssp.sh/brain/plaintext-files) files with some formatting sugar called [Markdown](http://ssp.sh/brain/markdown). I use Vim-motions heavily to make creating notes second nature for me (on a computer, at least).  Everything is optimized to improve my workflow and with the lowest barriers possible.

At a high level, we'll talk about how my workflow ultimately provides me a "[Deeper Life](http://ssp.sh/brain/deep-life)", which I'd like to call it, as it is less about business or any other specific use cases but all about your life and [Second Brain](http://ssp.sh/brain/second-brain). Although it will eventually lead to better careers, studies, and life too, as I have notices for myself over the years. therefore the term deep life.

# My Note-taking Path
Again, all this didn't happen in a couple of months or a year. This happened over many years, even the over two decades of my professional career, starting with [Microsoft OneNote](http://ssp.sh/blog/tools-i-use-onenote-part-ii/) and constantly improving file structures on my computer.

To give you some perspective, below you see how my path with note-taking proceeded to this day:
1. Forgetting everything
2. Taking scattered and very detailed notes on multiple devices, apps, and paper
3. Improving during my studies with OneNote, where notes related to work or study go into separate notebooks.
4. Starting to create a personal notebook for travels, research related outside of work, etc. But there is still a lot of confusion about:
	1. where to store my notes
	2. changing of the folder structure
	3. finding older notes is complex and rarely happened
5. Switching to **Obsidian** with a new open format and a different spirit and capabilities
6. Starting my [Second Brain](http://ssp.sh/brain/second-brain)
	1. Constantly updating my long-time wealth of personal knowledge by adding notes about my health, journals, cooking, books I read, and everything related to my life.
	2. I started to connect notes and sophisticate my system in a way that I confidentially find it later down my life span, the moment I need it
7. Start using [Vim](http://ssp.sh/brain/vim) and, more importantly, its [motions](http://ssp.sh/brain/vim-language-and-motions) for fast and effortless note-taking.
8. Sharing them publicly with [Quartz](http://ssp.sh/brain/quartz-publish-obsidian-vault).
9. Writing a [book](https://www.dedp.online/?ref=ssp.sh) with [MdBook](https://github.com/rust-lang/mdBook?ref=ssp.sh) on plain Markdown, shareing as I go as website.

# Why I Choose Obsidian?
I've written about [how to take notes](https://ssp.sh/blog/how-to-take-notes-in-2021/) and why I chose Obsidian over apps like Notion, Joplin, and Roam. The main reasons at that time were to have an open file format, coming from OneNote where the file format was proprietary, feeling the pain to get *my notes* out of that system (exporting it to HTML and converting them to Markdown, ..., see my scripts in [Python](https://github.com/sspaeti/second-brain-public/blob/hugo/utils/find-publish-notes.py?ref=ssp.sh), [Rust](https://github.com/sspaeti/second-brain-public/blob/hugo/utils/obsidian-quartz/src/main.rs?ref=ssp.sh), that was very important to me. I also mentioned how collaborating was a non-requirement for me.

If I reflect, I'm super happy about these choices, and I'm still confident, to this day, that my notes will forever grow with me. Even after Obsidian might die one day, as they are just simple text files with Markdown, they can be opened by any text editor in the past and future.

#### Why Would I Still Choose It Today?
> Today, I'd add the ability to **find knowledge whenever needed**. Confidentially storing ideas or notes, knowing I'll see them when needed, even years later.

The ability to **search based on a thought**. E.g., I forgot the note or a place, but I know the person who told me, so I searched for the persona and found the backlink to the place. As this is so close to how our brains work, this works so well for me, and I rarely search through the folder structure, except for recurring "area notes" based on the [PARA](http://ssp.sh/brain/para) method, which are constants notes such as family, house, health, etc.

**PARA and [Zettelkasten](http://ssp.sh/brain/zettelkasten)** are two more key players in my knowledge workflow. PARA that I have a minimal file structure that makes sense to me (it was already almost the one I optimized for myself over the year, but it added more sense and explained it more sophisticated). And the Zettelkasten way, that I do not need to spend a thought on where to store my note as one note can potentially belong to many different areas of my life, work, studies, therefore spending time where to store so I can find it later, took a lot of effort. But nowadays, I create a note in my Zettelkasten, which I can easily find with the above-mentioned search.

If I can't find a note with one search or it's missing a keyword, I add that searched keyword to the note, and Obsidian will update all links automatically. Next time I search and use the same initial keyword, I will find that note immediately. Also, for notes that appear highly searched, I will make them easier to search by updating them with more connections or adding more keywords to the title to find them immediately.

Moreover, Obsidian gives me the power to use **Vim motions**. This means I can use the shortcuts and mouse-free navigation that I learned and optimize it for coding and writing, spending almost no effort in clicking around and navigating trough my notes. Obsidian also makes it super easy to add shortcuts to any of the available commands. I am optimizing Obsidian-specific shortcuts and integrating them into my existing workflow

Lastly, everything is based on [Plaintext Files](http://ssp.sh/brain/plaintext-files) and [Local First](http://ssp.sh/brain/plaintext-files), with an additional hidden folder called `.obsidian`, which I used for Obsidian to store some metadata.

# How I Create Initial Notes (Templates)
It always starts with a template. With `.cmd+t` on Mac, I choose a Template. My default is `🌳Permanent Note Template`, which contains the following content:
```Markdown
# <% tp.file.title %>

---
Origin:
References:
Tags: #🗃/🌻
Created: <% tp.date.now("YYYY-MM-DD") %>
```
It will automatically file the title and the created date. I will then add the `Origin` so I know what triggered this note. I will add `References` of they connect to an existing note that immediately comes to mind. Usually, I leave this empty in the beginning but add at least one link with `[[]]` within the text.

For example, I will explain the term or the note I started, and add some rapid thought that might started that note.

Let's say I write a new open-source data ingestion tool. I will say something like, `This is similar to AirByte`, and  add the ingestion tool and its definition and features to the text. Usually, I will also add a [Map of Content (MOC)](http://ssp.sh/brain/map-of-content-moc) with all tools listed (e.g., [BI-Tools](https://ssp.sh/brain/bi-tools)), but if not, I can also find it via the backlink of AirByte in case I need to remember the name of it. As Airbyte is the most significant open-source ingestion tool, this will always come to mind, and I know I have connected it to it.

Tags can have these different levels:
- 📬 Start any note, idea, something I read, or anything that comes to mind. Just some fleeting notes. This can also be deleted after a while.
- 🗃/🌻I worked on it a bit. I added many fleeting notes, brainstormed, elaborated a bit, and made some references.
- 🗃/📖 literature notes written and note ready for the [Permanent Notes](http://ssp.sh/brain/permanent-notes)/Evergreen Notes. Still, [Literature Notes](http://ssp.sh/brain/literature-notes) are formulated in whole sentences and have already worked, or I'm just happy with the content.
- 🗃/🌳Evergreen/Permanent Notes. These long-running notes will end up in [Zettelkasten](http://ssp.sh/brain/zettelkasten) core with my own words. Here, I separated the literature notes into different ideas to follow the Zettelkasten principle and link them together.

This way, I can easily find different levels and quality of my notes, Evergreen being the best, in case I want only well-edited and long-running notes and hide freshly generated ones. See all of my tags in [Taxonomy of note types](http://ssp.sh/brain/taxonomy-of-note-types).
