# Portfolio build log: How I built a decentralized photo site *(and accidentally became a full-stack developer)*

How I skipped expensive web builders, ran into a wall of code (bugs), and ended up building a custom multi-server system to host my photography lookbooks for $0 (with absolutely no former web dev experience or formal training/education at all)

---

## The stack explained

Instead of paying a monthly fee to Squarespace or Wix (expensive!!), I built this by hand (AI-assisted, of course) and split it into separate pieces that don't rely on each other, so it costs me basically nothing to run:

* **The Looks:** Written in raw HTML using the **Tailwind CSS Play CDN** link so I can slap design styles onto the page instantly without dealing with annoying local terminal setup.
* **The Web Hosting:** Pushed to **GitHub** and automatically launched on **Netlify**. Every time I save my code, Netlify updates the live link globally.
* **The Photos:** High-res photography files are too heavy for GitHub and free cloud limits. To put that in perspective, the average size of a full-size a7RIII .jpg file is around *50-80 MB*, so it will choke out Git if I don't, especially as I am going to expand this website. Instead, they live on a physical, legacy **HP Compaq Linux server** in my room and stream to the website over an encrypted tunnel using **Tailscale Funnel**.

---

## How the whole thing is wired up

(Quick note first: I somehow got hold of an old PC that someone was literally giving away for free, and turned it into a home server. That's the backbone of this whole setup.)

There are three pieces and they each live in a different place:

The code sits on **GitHub** — this exact repo you're reading. It only holds the code, no photos or anything heavy, so it stays small and fast.

The live site runs on **Netlify**. It watches the repo, and every time I push a change it grabs the index.html and updates the live site in a few seconds. I don't have to do anything else.

The photos sit on my home server — that free **HP Compaq** running **Linux Mint**, in my room, switched on 24/7. All the heavy image files go there instead of GitHub, and I poke at it over SSH straight from my main laptop.

**Tailscale** is the bridge between the two. It links the site and the server through a private tunnel, so when the website needs a photo it reaches into the server and pulls it straight to whoever's looking. (tl;dr: it's the thing that lets my website and my random bedroom PC talk to each other.)

---

## The process (and the mistakes)

###  Stage 1: I want a photography portfolio website for myself
I realized I have a bunch of photos lying around, a professional, battle-tested photography skill, and absolutely no client at all. I realized I need a photography portfolio website, and I want it to be a high-contrast, matte black portfolio. I also need the thing to go online and be functional (while looking like an actual art portfolio and not a PowerPoint slideshow). I spin up Gemini and tell it to spit out an .html file, which is, of course, quite minimalistic and buggy. I tried to experiment with the code and swap variables until I arrived at something I am quite satisfied with. 

###  Stage 2: Backend
I found out that pushing massive 42-megapixel photos directly to a GitHub repository would completely choke it. The plan here is that I will use my spare Linux server that I picked up for free somewhere random and enter it via SSH, drop the image folder onto its local storage, and configure a secure Tailscale internet mesh so the Vercel(Netlify) frontend can grab the images directly from my physical server. Quite solid and doable, as I have Tailscale operational on all my devices already, so figuring this out wouldn't be so hard, as Tailscale is pretty easy to use anyway. 

###  Stage 3: The mess
As the grid grew as I kept adding stuff, I realized the code got absolutely bloated with endless duplicate utility classes, which I got pretty uncomfortable with because I can't keep track of 1000 <div> classes by eye without slowing down my deployment (remember the goal is to get this going as soon as possible). I got angry and tried to lazily copy-paste the code into a visual drag-and-drop editor (Pinegrow) to skip the coding work, but the entire page broke and made my portfolio look exactly like a PowerPoint slide show. Turns out, Pinegrow can't read live web scripts like the Tailwind CDN. 

To make it more confusing, I was trying to fix it inside `github.dev` in a web browser tab, wondering why I couldn't install live visual preview extensions. Then I checked Vercel, and it gave me a massive "No Domains" alert, making me think the whole deployment was dead (spoiler: of course it wasn't; I was just looking at the paid domain store instead of my project card).

To make it even worse, Vercel starts acting up and wouldn't let me to deploy my repo on it (see below). I eventually gave up and used Netlify instead, and it took around 2 minutes to set up. This makes me feel slightly stupid because why do I do it this late? 

### Stage 4: Settling down and finishing
Cleaned up the mess. Pulled the repository out of the browser and locally cloned it onto my main computer's desktop version of **VS Code**. Installed the **Live Server** extension so hitting save updates my local monitor instantly, giving me a zero-latency feedback loop. Found out that Tailwind Visual Editor doesn't work for some reason. Continuing soon. 

---

## How the photo grid works now (and why I stopped hand-coding every photo)

Remember the Stage 3 mess, where the page got bloated with a thousand duplicate `<div>`s I couldn't keep track of? Here's how I actually killed that for good, instead of just cleaning it up and waiting for it to come back.

The old way: every single photo was its own lump of copy-pasted HTML. Add a photo, copy the block, swap the link, swap the title, and pray I didn't break a class somewhere. Do that enough times and it turns into a swamp.

So I split it in two:

* All the info about a photo — image, title, category, credit — is now just one line in a list. One photo, one line.
* The actual look of a card (all the Tailwind classes, the hover stuff, the layout) is written once, as a single template. A bit of JavaScript runs down the list and spits out a card for each photo on its own.

Why I bothered:

* Adding a photo is one line now. I drop it in the list and it shows up. No more digging through a wall of identical divs to find the right one.
* If I want to change how every card looks, I change that one template instead of editing every single photo.
* Each photo carries its own category, so the filter buttons can't fall out of step with what's actually on the page.
* When I finally hook the site up to my home server, I change two little helper lines and every image points at the server — I don't have to touch them one by one.

The catch (there's always one): the grid gets built by JavaScript now, so it isn't sitting in the raw HTML anymore. A Google bot, or someone browsing with JavaScript switched off, would just see an empty grid. For a photo site that every actual human opens in a normal browser, I genuinely don't care — but that's the trade, and I can deal with it later if it ever matters.

And the categories: I split everything into four — **Portrait, Urban, Landscape, Still Life** — by what's actually in the photo, not by who paid for it. Sorting by "commercial vs not" is pointless when basically everything I shoot is commercial anyway, so that tells a visitor nothing. Four plain words, each clearly different, and no lazy "and Other" bin that just means I couldn't decide where something goes.

---

##  How I actually built this

I actually have no experience at all in web development, so ... I basically used the AI as a sounding board (translate: I had the vision in my head, and I'd feed it to the AI and get it to tell me how to actually pull the details off) to figure out the technical stuff on the fly:

Network setup: Figured out I can stitch a cloud site (Vercel, Netlify?) together with a random old computer I got for free and turned into a Linux server. Hooked them up using a Tailscale tunnel so I can host heavy photos for free without choking GitHub.

The Pinegrow mess: Tried using a visual drag-and-drop editor to save time, but it completely fucked up my fonts and theme because offline apps can't parse live web scripts like the Tailwind CDN. 

The browser vs. desktop confusion: Got stuck trying to install local development tools inside a browser tab on github.dev before realizing I needed to quit messing around in the web browser and move to actual native VS Code.

The Vercel issue: Opened the Vercel dashboard and saw a flashing "No Domains" warning, briefly thought I nuked my entire project, but later realized I was just looking at their paid domain shop instead of my actual project link. Also tried to solve the whole saga revolving around it, ultimately resulting in me throwing it away completely. 

The command palette thingy: Realized VS Code wasn't finding the Git tools because the search bar was missing the > command symbol. Caught the missing character myself, forced the editor to run the clone command, and finally yanked the code down from GitHub.

### A bunch of random questions I asked Gemini (updated regularly) (some very funny, others are not)

Night 2:
* why is my whole HTML page broken?? (turned out a copy-paste error turned `class=""` into `ass=""` — quite funny I think)
* how does the Tailwind CDN link just instantly load all the design styles into the browser, and why does that make Pinegrow unable to see any of it?
* what's the actual difference between committing straight to the live `main` branch vs spinning up a separate branch and opening a pull request?
* is a Git "commit" just a save file, or is it something more permanent? why can't I just delete one?
* how does a `mailto:` link open my email app instead of taking me to a website?
* how does VS Code's Live Server turn a normal Chrome tab into a live preview that updates the second I hit save? (what's port 5500 about?)
* why the hell won't the Tailwind Visual Editor work in my fucking VS Code
* why does pushing huge files to Git choke it so badly? (just asking for fun, I already have my home server)

Day 3: 
* what does it mean when Git says "your branch and 'origin/main' have diverged", and how did I cause it?
* what does "nothing to commit, working tree clean" actually mean when it pops up in the terminal?
* does a force push (--force) fix the diverged-branch thing, and how badly can it screw me over?
* why did my push go through fine but the live site still shows the old version?
* what's the best way to work across my laptop and the browser without my branches diverging every single time?
* why does my `mailto:` link seem dead and do nothing when I click it on desktop?
* why does the browser sometimes not show my latest edits even with Live Server running?

### Takeaway:
Instead of wasting nights typing out a hundred individual code brackets by hand, I used the AI like my personal code monkey (and assistant). As in, I handled the system architecture, network tunnels, and visual art direction, and let the AI do the heavy lifting on the syntax and overall coding while also asked it whatever I don't know or not sure. Built the entire pipeline during a single-week sprint in mid-May.

