# Portfolio build log: How I built a decentralized photo site *(and accidentally became a full-stack developer)*

How I skipped expensive web builders, ran into a wall of code bugs, and ended up building a custom multi-server system to host my photography lookbooks for $0 (with absolutely no former web dev experience or formal training/education at all)

---

## The stack explained

Instead of paying a monthly fee to Squarespace or Wix, I built this layout by hand (AI-assisted, of course) and decoupled the pieces so it costs absolutely nothing to run:

* **The Looks:** Written in raw HTML using the **Tailwind CSS Play CDN** link so I can slap design styles onto the page instantly without dealing with annoying local terminal setup.
* **The Web Hosting:** Pushed to **GitHub** and automatically launched on **Vercel**. Every time I save my code, Vercel updates the live link globally.
* **The Photos:** Raw, high-res photography files are too heavy for GitHub and free cloud limits. Instead, they live on a physical, legacy **HP Compaq Linux server** in my room and stream to the website over an encrypted tunnel using **Tailscale Funnel**.

---

## The process (and the mistakes)

###  Stage 1: I want a photography portfolio website for myself
I realized I have a bunch of photos lying around, a professional, battle-tested photography skill, and no client at all. I realized I need a photography portfolio website, and I want it to be a high-contrast, matte black portfolio. I also need the thing to go online and be functional (while looking like an actual art portfolio and not a PowerPoint slideshow). 

###  Stage 2: Backend
Realized pushing massive 42-megapixel photos directly to a GitHub repository would completely choke it. Grabbed my spare Linux server that I picked up for free somewhere random and entered it via SSH, dropped the raw image folder onto its local storage, and configured a secure Tailscale internet mesh so the Vercel frontend could grab the images directly from my physical server.

###  Stage 3: The mess
As the grid grew, the code got absolutely bloated with endless duplicate utility classes, which I got pretty uncomfortable with because I can't keep track of 1000 <div> classes by eye without slowing down my deployment (the goal is to get this going as soon as possible). I got angry and tried to lazily copy-paste the code into a visual drag-and-drop editor (Pinegrow) to skip the coding work, but the entire page broke and stripped my fonts. Turns out, offline visual apps can't read live web scripts like the Tailwind CDN. 

To make it more confusing, I was trying to fix it inside `github.dev` in a web browser tab, wondering why I couldn't install live visual preview extensions. Then I checked Vercel, and it gave me a massive "No Domains" alert, making me think the whole deployment was dead (spoiler: of course it wasn't; I was just looking at the paid domain store instead of my project card).

### Stage 4: Cleaning
Cleaned up the mess. Pulled the repository out of the browser and locally cloned it onto my main computer's desktop version of **VS Code**. Installed the **Live Server** extension so hitting save updates my local monitor instantly, giving me a zero-latency feedback loop.

---

##  How I actually built this

I actually have no experience at all in web development, so I use Gemini to help me figure out the following: 

The Network Setup: Figured out how to stitch a cloud site (Vercel) together with a random old computer I got for free and turned into a Linux server. Hooked them up using a Tailscale tunnel so I can host heavy photos for free without choking GitHub.

The Pinegrow Mess: Tried using a visual drag-and-drop editor to save time, but it completely fucked up my fonts and theme because offline apps can't parse live web scripts like the Tailwind CDN. 

The Browser vs. Desktop Confusion: Got stuck trying to install local development tools inside a browser tab on github.dev before realizing I needed to quit messing around in the web browser and move to actual native VS Code.

The Vercel Heart Attack: Opened the Vercel dashboard and saw a flashing "No Domains" warning, briefly thought I nuked my entire project, but later realized I was just looking at their paid domain shop instead of my actual project link.

The Command Palette Friction: Realized VS Code wasn't finding the Git tools because the search bar was missing the > command symbol. Caught the missing character myself, forced the editor to run the clone command, and finally yanked the code down from GitHub.

###### Takeaway:
Instead of wasting a whole night typing out a hundred individual code brackets by hand, I treated the AI like my personal code monkey. As in, I handled the system architecture, network tunnels, and visual art direction, and let the AI do the heavy lifting on the syntax. Built the entire pipeline during a single-week sprint in mid-May.
