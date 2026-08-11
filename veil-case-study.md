# Collaborating with Claude to build Veil, a bachelorette trip planning app, from concept to interactive prototype

## Callout
- **Objective:** Create an app I'd actually use in collaboration with Claude from inception to design to interactive prototype.
- **Result:** Full MVP spec, a three-theme design system in Figma, and a working demo with role and theme toggles that demonstrate the app's signature feature.
- **My Role:** Solo designer and builder. Claude as design systems thought-partner and code execution partner.
- **Tags:** AI-assisted build · Mobile Design · Design Systems · End-to-end delivery

[ASSET: Hero mockup. Three iPhones side by side showing the same day of the itinerary in Planner / Bride / Guest view, so the hidden-from-bride invariant is visible at a glance. A short looping video of the role toggle flipping between the three states would be even stronger.]

---

## I was procrastinating planning my best friend's bachelorette so I started thinking about apps I could use to manage it easier

I wanted a project that would push me into mobile design and give me a portfolio case study. My first idea was a habit tracker with a color-as-data aesthetic. I sat with it for a while and realized I had nothing new to add to a category that already has hundreds of good apps. So I shelved it and waited for a real problem.

The problem showed up when I got tapped as MOH for my friend Nidhi's bachelorette in Scottsdale. Splitting expenses across five apps, coordinating surprises in a side text thread the bride wasn't in, keeping the itinerary in a Google Doc nobody read. All of it was the kind of chaos that makes a real design problem obvious. The bride wanted to help plan, but half of what we were planning was for her. There was no tool that respected that.

[ASSET: A photo of your actual planning chaos if you have it (screenshots of a messy group chat, a Google Doc, a Splitwise ledger). Alternatively, a simple diagram showing the "5 apps → 1 app" collapse.]

---

## The signature idea: one shared plan, seen differently by three roles.

I explored the concept with Claude in a long back-and-forth where I basically just threw out all my ideas and asked it to organize my thoughts. While planning the experience and reflecting on how each person in the bridal party would view it, it became clear that there are three roles: the Planner (usually the MOH which is me), the Bride, and the Guest. They all see the same underlying trip data. What differs is what gets filtered.

The hidden-from-bride rule runs across every surface of the app. Hidden events vanish entirely from her itinerary so there's no suspicious gap. Teased events show as a mystery card that holds the time slot. Secret expenses drop out of her ledger before the settle-up math runs, so her total never leaks a surprise. Announcements from bride-excluded boards carry a "secret" tag so nobody blabs.

I named the app Veil. It's bridal, and it literally means to conceal.

[ASSET: A diagram of the three-role visibility system. Same day, three columns, showing what each role sees. This is the money shot of the whole project and should be prominent.]

---

## I wrote the spec by vomiting ideas at Claude.

I would dump every thought I had about the app into the chat, and Claude would organize it into a structured spec doc, flag gaps in workflows where I hadn't thought through the details, and push back where things were contradictory.

That format worked because it matched how the ideas actually came out of my head, which is not in the order they belong in a spec.

The tough part throughout was scope creep. Every session I would think of another feature that seemed necessary or cool to have, stemming from the conversations I was having with my bride about our actual trip. Polls with weighted bride votes, pooled budgets, outfit color extraction, room assignments from comfort polls, a shared idea board. Claude was helpful here for pointing out which additions would be low lift and easy to bundle together versus opening a whole new can of beans, and for talking me through what got cut versus parked. Everything trimmed went into a Future section of the spec so the vision stayed visible without pulling the build off scope.

The MVP settled at four tabs. Itinerary, expenses, messaging, account.

[ASSET: A screenshot or clean recreation of the spec's table of contents, or the horizontal flow map (bach-planner-flow.html) that shows onboarding forking into the four-tab app.]

---

## Claude tried to jump straight to prototyping, but I wanted to sketch first.

Two batches of hand sketches on dot grid paper, covering essentially every screen in the MVP. Onboarding, the itinerary in all three role views, the Add Activity form with the visibility toggle, the expense settle-up flow, the messaging boards with the "hidden from THE BRIDE" ghost bubble.

Sketching this loose let me catch logic problems early. The bride's timeline with a hidden event needed to not have a gap where the event was. The teased event needed to hold the time slot as a mystery block. The "bride is paying for herself" checkbox on Add Activity couldn't logically coexist with a hidden event. Reading through the spec as I drew helped point out all the flows I needed to outline and find edge cases for.

I walked Claude through the sketches after each batch and used the review to resolve open questions in the spec.

[ASSET: A grid of your sketches, or a couple of the strongest pages (the itinerary in three states, or the Add Activity form with the visibility control).]

---

## I handed Claude the sketches and asked for a component list.

Before building anything in Figma, I gave Claude all the sketches and asked it to reason backward from them: what's the minimum set of components I need to build every screen in this app?

Claude came back with a checklist organized into foundations, atoms, signature components, and structural pieces. It correctly identified the activity card as the piece to spend the most care on, since it has to carry the visibility system's meaning in every state. It flagged the visibility control itself as a signature component that would touch every surface. It listed states (default, focused, filled, disabled, selected) rather than just resting looks, which is how I created and organized the components and variants as I built them.

That checklist became my build queue. I used it verbatim, checking things off as I built them, so I could focus on drawing components instead of deciding what to draw next.

[ASSET: A screenshot or clean rendering of the component checklist, with items checked off.]

---

## I designed the token system, Claude built it in Figma.

The theme is a property of the weekend, chosen once at group creation. Everyone in the group inherits it. This let me commit to a specific aesthetic per trip without splintering the component library.

I built three themes: Desert Warmth (the initial inspiration from my actual bach trip in Scottsdale), Playful Pop (something more modern), and Ivory Veil (very classic "bridal").

The token architecture is two collections. Primitives holds raw values across three theme modes. Semantic holds single-mode aliases into Primitives. Components only ever reference semantic names like accent, surface-card, visibility-hidden. Switching themes swaps the Primitives mode, so as long as everything is pointing to the same semantic use of a color, it can fully shift to another theme with minimal issues.

I chose the architecture and the values. Claude executed the setup in Figma via MCP: creating the collections and modes, wiring every primitive variable across every theme, aliasing the semantic tokens, and building the text styles per theme. This saved me hours of manual variable-creation clicking and let me stay in the reasoning layer while the tokens got assembled underneath.

Claude also caught the systems-level things I would have been annoyed testing myself. I noticed quirks in the text styles and expressed concern on contrast and readability between accent and text colors. It ran WCAG contrast audits on the Ivory Veil buttons (the gold on white failed until we darkened the ink) and spotted that Cormorant's oldstyle figures were misaligning numbers in the UI, which is why all numerics in the app route through DM Sans.

[ASSET: A three-column color swatch showing the same semantic tokens resolving to three different values across the themes. Followed by a screenshot of the same Home screen rendered in all three themes.]

---

## I built the components and a few base sample screens by hand.

Working from the checklist, I built each component once in Desert Warmth and let the other two themes inherit through the token layer. Buttons, chips, inputs, avatars, the visibility control, event cards in five variants (default, half-secret, teased, secret, past), expense rows, announcement messages, the timeline hour rail, the bottom nav.

The activity card is the most-worked component. It has five variants because it has to communicate the same event's visibility state differently depending on which role is looking. A teased event looks different in the planner's view (real details plus a lock badge) than in the bride's view (a mystery card with the time slot but no content).

I also laid out sample screens for the surfaces I most wanted to nail visually, and troubleshot the styles and spacing that weren't quite reading right. That gave me a strong reference set to hand off, and let me stop wrestling with visual detail before it derailed the build phase. The main goal was to express how I wanted components to fit together in a screen and set specific parameters for padding and information architecture without having to build out the whole app myself. I just wanted to make enough for the point to get across and Claude could extrapolate and fill in the rest of the screens and gaps without me needing to go back into Figma to make more reference material.

[ASSET: A screenshot of the component library page in Figma, ideally the activity card variants laid out together.]

---

## Claude Code built the rest of the prototype, and we iterated together as I noticed edge cases and troubleshooted interactions.

Once components and reference screens existed, I moved to Claude Code and had it pull straight from Figma via MCP to build the remaining screens I hadn't fully laid out. Claude filled in the interaction gaps I hadn't specified in the sketches, added animations, and linked every screen together into a real navigable prototype.

The architecture matters here. The role filter runs on the seed data before anything renders. There's no clever conditional CSS hiding secret cards from the bride. Her copy of the data literally does not contain them. That's the same invariant the real backend would have to hold, and it means the demo doesn't cheat.

I stayed the design lead throughout. Claude wrote most of the code. I directed the architecture, caught the fidelity issues (missing card borders, wrong badge placement, redrawn icons instead of using my exports, etc), and made every product call. The working agreement was strict literal fidelity: carry exact structure from the Figma pulls, no reorganization, use my real assets.

[ASSET: A GIF or short video of the role toggle in action on the Home screen. Show a hidden event vanishing when the bride is selected, then reappearing for the planner. This is the demo moment.]

---

## The MVP prototype is done.

Every surface in the MVP is built, seeded, and interactive. The role toggle flips between Planner, Bride, and Guest with a live re-render so you can watch a hidden event disappear from the bride's timeline and a secret expense drop out of her ledger. The theme toggle switches the entire app between Desert Warmth, Playful Pop, and Ivory Veil, exercising the token system end to end.

[ASSET: The strongest asset for this section is a longer walkthrough video of the finished prototype. Show the role toggle, the theme toggle, and one full flow (Add Activity, or add-and-approve an expense). Or embed the live prototype if it's hosted.]

---

## Working with Claude changed how I scope a solo project.

Two things stand out.

First, the spec was written faster and tighter than I would have written it alone. Claude was good at catching contradictions ("if the bride is paying for herself, can she also not know the event exists?") and pushing me to resolve them before I built the wrong thing in Figma. The scope-creep battle was easier to win with someone else in the loop who could tell me when a new addition was going to require an unnecessarily high lift or open a new can of beans.

Second, the design system reasoning was a genuine dialogue, and the execution was fast because Claude could act directly inside Figma. I made every call. But having a partner to run a WCAG audit on the spot, wire up all my variables while I moved on to the next decision, or build out screens I'd only sketched, meant the boring parts got done in the background while I stayed focused on the design work and problem solving, which is what I actually like.

**The hand-drawing, the visual direction, the component construction, and every product decision were mine. Claude was the systems collaborator and the build executor.**

---

## In an ideal world, it would be dope to make this a real app for use for this upcoming bach and all my other bride friends.

Ship it so it's actually usable for a real trip. But that means a real backend behind the invariant that today lives only in the seed data, and everything that comes with turning a demo into a product. If you have any feedback on what you've tried, please [email me](mailto:raoshalinid@gmail.com)!

After that, start layering the features I parked in the Future section: polls with weighted bride votes, richer expenses with pooled budgets, outfit and theme tooling, room assignments, the shared idea board. The design system and the role model are already built to hold them.

[ASSET: If the prototype is hosted, link it prominently at the end. Same for the Figma file if you want to make it public.]
