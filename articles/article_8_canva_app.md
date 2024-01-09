# Creating Canva Apps: A Python Developer's Perspective

## Overview

Unlike my usual focus on Python development, this article explores creating apps for Canva. I enjoy making apps that
people can actually use. In the past, I've built stuff for Slack, Telegram, Zoom and Discord. This time, I wanted to try
my hand at Canva App Development.

For those unfamiliar, Canva is a super easy graphic design tool. Just check it out at https://www.canva.com. Now, Canva
has this cool marketplace (https://www.canva.com/your-apps) where you can add extra features to your design tool.

The app itself is written in TypeScript & React, using the Canva SDK. What's neat is that it can connect to your own
backend, and guess what? You can write that backend in Python! So, even though we're talking about Canva App Development, there's
still a little bit of Python involved.

For more details on Canva App development, you can check out their documentation here: https://www.canva.dev/docs/apps/

## Unveiling My First Canva App: ASCII Text Fun!

Let's dive into the tale of a joint project with Alex (https://github.com/kobuqa) as we wade into the realm of Canva App
Development. It's been an enlightening and collaborative experience from the get-go.

No need for a grand reveal, but I'm delighted to share the release of my debut Canva App - "ASCII Text." Find it
casually mingling on the Marketplace (https://www.canva.com/your-apps/AAF1BNdhFeM/ascii-text?q=ASCII+Text).
Take a laid-back peek at ASCII Text Canva App.

For the inquisitive minds out there, additional details about the app are just a click away. Explore all the
nitty-gritty info on the ASCII Text App Info page (https://omer.duskin.me/ascii-text).

Now, let's spill the beans on the behind-the-scenes magic. This Canva App isn't just code; it's a creative adventure
designed to sprinkle a bit of fun into your Canva experience.

## The Development Journey

Having crafted applications across various platforms, I must commend Canva for its developer-friendly approach. The
process of creating a basic application on Canva is remarkably straightforward.

For those eager to delve into Canva app development, the Canva documentation serves as a valuable
guide (https://www.canva.dev/docs/apps/).

Now, let's walk through the steps of bringing your Canva app to life:

1. Canva App Submission:
   Begin by submitting your Canva app. The initial step sets the stage for the journey ahead.

2. Functional Review:
   Expect a comprehensive functional review that takes approximately a week. This crucial step ensures your app aligns
   with
   Canva's standards.

3. Design Review:
   The design review phase follows, ranging from 1 to 4 weeks, depending on their queue. This meticulous examination
   ensures your app integrates seamlessly with Canva's design principles.

4. Pending Lead Approval:
   Once the reviews are complete, your application awaits lead approval. This step ensures all aspects align with
   Canva's
   vision.

5. Application Approved:
   Congratulations! Your Canva app has been approved, marking the successful completion of the development journey.

Canva's structured process not only simplifies development but also ensures the quality and coherence of applications
within their ecosystem. Explore the possibilities and bring your creative ideas to life on Canva!

## Getting Started with Canva App Development

If you're ready to dive into Canva app development, you're in for a treat. The app is written in React, leveraging the
user-friendly Canva SDK. Here's a simple guide to get you started:

1. Canva SDK Setup:
   Head over to Canva Developers to access the getting-started guide. It's your gateway to the Canva SDK, and it's
   remarkably easy to follow.

2. Clone the Starter Kit:
   Begin by cloning the canva-apps-sdk-starter-kit repo. This sets the foundation for your Canva app development.

3. Start Building:
   The cloned repo contains everything you need to kickstart your development journey. Follow the straightforward
   instructions to begin crafting your Canva app.

## Streamlining Development for Multiple Apps

While the starter kit is fantastic for a single app, what if you're itching to create multiple applications or prefer
keeping your app code in a different repository? Here's a neat approach:

1. Organize Your Code:
   Create a new repo specifically for your Canva Applications code. Each application gets its dedicated folder within
   this repository.

2. Custom Development Setup:
   Within each app folder, craft a start.sh file with commands to copy your .env and link your app.tsx to the starter
   kit. This ensures a smooth development process.

```shell
cp ./.env ../../canva-apps-sdk-starter-kit/.env && \
ln -f ./app.tsx ../../canva-apps-sdk-starter-kit/src/app.tsx && \
cd ../../canva-apps-sdk-starter-kit/ && \
npm start
```

3. Configure Package.json:
   In each app folder, set up a package.json file with the necessary scripts.

```shell
{
  "type": "module",
  "scripts": {"start": "source start.sh"}
}
```

By adopting this approach, you can seamlessly work on different apps, pushing only the relevant code into your
repository. This decoupling from the starter kit allows for a flexible and organized development process. Happy coding!

## ASCII-TEXT Canva App Overview

In my exploration of Canva App Development, the first gem I created is ASCII-TEXT. You can check out all the details
about it here (https://omer.duskin.me/ascii-text).

To spice up Canva designs, I seamlessly integrated the Figlet package into the app. This empowers Canva users to
effortlessly add captivating ASCII Text Art to their creations. It was truly a delightful experience, and while I hope
users find value in it, the primary motivation was the sheer joy of the development process.

My journey in app creation has seen me crafting solutions for platforms like Slack, Telegram, and now,
the curiosity led me to Canva. It's not just about the end result; it's about the journey and learning along the way.

For those interested in the nitty-gritty of the code, you can explore the app's repository. The Canva app is neatly
compiled into a single JS file. To keep it lean, I opted to import only a selection of Figlet fonts. Alternatively,
using the Figlet Package in the backend is another avenue. A lightweight backend, perhaps an AWS Lambda function, can
efficiently handle fetching the requested font and delivering the ASCII text as a response. While this approach reduces
package size and supports all available fonts, it's essential to weigh it against factors like response time and
computing costs—both of which, in this context, are expected to be minimal.

Feel free to explore, experiment, and perhaps add a touch of ASCII creativity to your Canva designs!

## Navigating the Canva App Approval Journey

Getting your app approved on Canva involves a two-step dance: the Functional Review and the more intricate Design
Review. Let's waltz through the process and the challenges encountered.

### Functional Review: A Quick QA

This initial stage is all about functionality. Canva wants to ensure your app works seamlessly without any bugs. It's
essentially a user-centric QA, and the response is relatively swift – usually within a few days. If there are hiccups,
they'll send detailed feedback, and you'll need to submit a revised version with the fixes. Think of it as the first
draft – quick and crucial.

### Design Review: Where the Real Dance Happens

Now, let's talk about the more challenging Design Review. Brace yourself; this part takes longer, sometimes a few weeks.
It's a meticulous examination of your app's aesthetics. If you ever wondered why Canva Marketplace apps share a similar
look-and-feel, it's not by accident. The Design Review ensures a cohesive visual experience, focusing on spacing, UI
components, colors, and UX principles.

In case of rejection, fear not! The feedback is comprehensive, featuring a detailed breakdown of issues along with
screenshots of expected designs. It's not a generic rejection; Canva invests effort to align your app with the current
design standards. They might even go the extra mile, providing video explanations for some issues.

Despite the occasional annoyance due to the timeline, the Design Review is an educational gem. It sharpens your
understanding of design principles and enhances the overall quality of your app.

### Design Suggestions and Marketplace Magic

The Design Review doesn't stop at feedback; it often comes with feature suggestions. In my case, they advised (not
mandated) adding a "Copy To Clipboard" feature, a delightful addition.

Here's a fun twist – they even helped me with some design concepts for the marketplace images. And guess what? I crafted
those using Canva. It's a testament to the tool's versatility and a full-circle moment in the Canva App approval
journey.

In essence, while the dance with Canva's approval process has its intricate steps, it's a valuable experience that
ensures your app not only works flawlessly but also looks dazzling in the Canva universe.

# Conclusion: A Canva App Journey Unveiled

In the realm of Canva App Development, the journey has been both enlightening and rewarding. Creating my debut app,
ASCII-TEXT, allowed me to seamlessly integrate the Figlet package, offering users a whimsical touch to their designs.

The approval process, divided into Functional and Design Reviews, provided insights into the meticulous standards upheld
by Canva. The Functional Review, akin to a quick QA, focused on ensuring the app's bug-free functionality, allowing for
rapid iterations. On the other hand, the Design Review unveiled the intricacies of aligning with Canva's design
principles, offering not just rejections but detailed feedback, suggestions, and even design concepts.

While the timelines and occasional challenges in the Design Review may test patience, the educational value is
undeniable. Canva invests in guiding developers, ensuring not just functionality but a harmonious visual experience for
users.

From the initial excitement of development to the dance of reviews, the Canva App journey is a testament to the
platform's user-centric approach and commitment to quality. Whether it's the thrill of approval or the valuable feedback
in rejection, each step contributes to a learning-rich adventure.

As I wrap up this journey, I look forward to more creations, more insights, and perhaps more ASCII art sprinkled across
Canva designs. The world of Canva App Development is dynamic, and with each iteration, it opens doors to new
possibilities and creative horizons. Cheers to the journey, the challenges, and the creativity that unfolds in the Canva
universe!







