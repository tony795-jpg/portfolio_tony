# 🛠️ Portfolio Build Log: How I built a decentralized photo site

A quick diary of how I skipped expensive web builders, ran into a wall of code bugs, and ended up building a custom multi-server system to host my photography lookbooks for $0.

---

## 🏗️ The System in Plain English

Instead of paying a monthly fee to Squarespace or Wix, I built this layout by hand and decoupled the pieces so it costs absolutely nothing to run:

* **The Looks:** Written in raw HTML using the **Tailwind CSS Play CDN** link so I can slap design styles onto the page instantly without dealing with annoying local terminal setup.
* **The Web Hosting:** Pushed to **GitHub** and automatically launched on **Vercel**. Every time I save my code, Vercel updates the live link globally.
* **The Photos:** Raw, high-res photography files are too heavy for GitHub and free cloud limits. Instead, they live on a physical, legacy **HP Compaq Linux server** in my room and stream to the website over an encrypted tunnel using **Tailscale Funnel**.

---

## 📈 The Journey (and the mistakes)

### 📍 Stage 1: The Idea
Wanted a completely clean, high-contrast matte black portfolio. Dropped a single script tag for Tailwind into an `index.html` file, linked it up to Vercel, and had a live web link functioning on the internet in minutes. 

### 🖥️ Stage 2: Moving onto Bare Metal
Realized pushing massive 42-megapixel photos directly to a GitHub repository would completely choke it. Set up a headless Linux box via SSH, dropped the raw image folder onto its local storage, and configured a secure Tailscale internet mesh so the Vercel frontend could grab the images directly from my physical server.

### 💥 Stage 3: The Part Where Everything Broke
As the grid grew, the code got sloppy with endless duplicate utility classes. Tried to lazily copy-paste the code into a visual drag-and-drop editor (Pinegrow) to skip the coding work, but the entire page broke and stripped my fonts. Turns out, offline visual apps can't read live web scripts like the Tailwind CDN. 

To make it more confusing, I was trying to fix it inside `github.dev` in a web browser tab, wondering why I couldn't install live visual preview extensions. Then I checked Vercel and it gave me a massive "No Domains" alert, making me think the whole deployment was dead (spoiler: it wasn't, I was just looking at the paid domain store instead of my project card).

### 🚀 Stage 4: Getting it Done
Cleaned up the mess. Pulled the repository out of the browser and moved it local—cloning it onto my main computer's desktop version of **VS Code**. Installed the **Live Server** extension so hitting save updates my local monitor instantly, giving me a zero-latency feedback loop.

---

## 💡 How I Actually Built This

Yes, I used **Gemini** to quickly generate the repetitive HTML layout blocks and the lightweight JavaScript for the image modal/lightbox popups. 

Instead of wasting a whole night typing out a hundred individual code brackets by hand, I treated the AI like an entry-level assistant—I handled the system architecture, network tunnels, and visual art direction, and let the AI do the heavy lifting on the syntax. Built the entire pipeline during a single-week sprint in mid-May.
