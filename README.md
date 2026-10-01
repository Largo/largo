### Hi there 👋

<!--
**Largo/largo** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

I'm Andi(@largo)! I like to automate things for myself and others, so I became a programmer! Please leave a comment or an issue if you try out any of my projects and cannot get it to work. I love meeting new people, say hello. お気軽にご連絡ください。

## A small part in making Ruby faster on Windows
In January 2023 I wondered why `require 'gtk3'` took 3+ seconds on Windows. I traced it with [Procmon](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) and filed [Bug #19378](https://bugs.ruby-lang.org/issues/19378). The cause: Windows has no native `realpath`, so Ruby emulates it. Every `require` opened a handle on **every parent directory** of the file, and each open cost 8 syscalls. One file deep inside a gem (`C:\Ruby32-x64\lib\ruby\gems\3.2.0\gems\glib2-4.0.8\lib\glib2\variant.rb`) needed **80 syscalls**, and the same work was repeated for every file.

In 2026 I tried `GetFileInformationByName` (Windows 11 24H2+), which gets the same information in one call, and attached a rough patch. [Hiroshi Shibata](https://github.com/hsbt) did the careful part: he reworked it for ReFS, junctions and mount points, tested it thoroughly and merged it in [ruby/ruby#18193](https://github.com/ruby/ruby/pull/18193). My [original commit](https://github.com/ruby/ruby/commit/a3c7ff63ea5ade187ec87d2344ca296a9e90cf64) is part of it. In his benchmarks `File.stat` got about 3.9× faster and requiring big gems like rubocop or active_support 1.35–1.55× faster.

This should be in Ruby 4.1 this Christmas, so the short delay when a Ruby program starts on Windows gets noticeably shorter.

I also tried YJIT on Windows. It had never run there ([#19325](https://bugs.ruby-lang.org/issues/19325)), and in August 2026 I got an [experimental port](https://github.com/Largo/ruby/pull/1) working with a lot of help from AI. It mostly showed that it could be done. hsbt then wrote a proper implementation for mswin ([ruby/ruby#19114](https://github.com/ruby/ruby/pull/19114)), which passes the test suite and runs about 2× faster than the interpreter in his benchmarks. It is waiting for review now.

Thanks to nobu, hsbt and everyone who commented on these tickets over the years.

## New this autumn (2026)
<table><tr>
<td width="33%"><a href="https://github.com/Largo/chunkybacon"><img src="https://raw.githubusercontent.com/Largo/chunkybacon/main/docs/social/twitter-card.png" alt="Chunky Bacon: learn Ruby in your browser"></a></td>
<td width="33%"><a href="https://github.com/Largo/ruby_llm-claude_cli"><img src="https://raw.githubusercontent.com/Largo/ruby_llm-claude_cli/main/docs/social/twitter-card.png" alt="ruby_llm-claude_cli: your Claude Code login as a RubyLLM provider"></a></td>
<td width="33%"><a href="https://github.com/Largo/ruby_llm-providers-infomaniak"><img src="https://raw.githubusercontent.com/Largo/ruby_llm-providers-infomaniak/main/docs/social/twitter-card.png" alt="ruby_llm-providers-infomaniak: Swiss-hosted open models in plain Ruby"></a></td>
</tr></table>

**Ruby in the browser (ruby.wasm)**
- [Largo/chunkybacon: Learn Ruby in the browser with Chunky Bacon](https://github.com/Largo/chunkybacon). An interactive notebook course in German and English, running on ruby.wasm.
- [Largo/jsg](https://github.com/Largo/jsg): the `jsg` command creates, builds and serves ruby.wasm browser projects. [Gem](https://rubygems.org/gems/jsg)
- [Largo/nokogiri-pure](https://github.com/Largo/nokogiri-pure): Nokogiri 1.19.4 in pure Ruby. libxml2, libxslt and gumbo ported to Ruby, no C extension, so it runs on ruby.wasm. [Gem](https://rubygems.org/gems/nokogiri-pure)
- [Largo/bigdecimal-pure](https://github.com/Largo/bigdecimal-pure): BigDecimal in pure Ruby. `require 'bigdecimal'` still uses the native gem when it is available. [Gem](https://rubygems.org/gems/bigdecimal-pure)

**Ruby + LLMs**
- [Largo/ruby_llm-claude_cli](https://github.com/Largo/ruby_llm-claude_cli): use Claude Code and its subscription login as a [RubyLLM](https://rubyllm.com) provider. Streaming, images, PDFs, Office files, structured output and tool calls, no API key needed. [Gem](https://rubygems.org/gems/ruby_llm-claude_cli)
- [Largo/ruby_llm-providers-infomaniak](https://github.com/Largo/ruby_llm-providers-infomaniak): a RubyLLM provider for Infomaniak AI Tools (Swiss-hosted models). [Gem](https://rubygems.org/gems/ruby_llm-providers-infomaniak)

**Office files**
- [Largo/ruby_pptx](https://github.com/Largo/ruby_pptx): a Ruby port of python-pptx. Create, read and update PowerPoint files. [Gem](https://rubygems.org/gems/ruby_pptx)

## Recent Projects
- [Largo/ocran: Turn Ruby Scripts into .exe files. Now with Linux and MacOS Support](https://github.com/Largo/ocran). Cosmopolitain LibC Support means that a simple Rails app can be shipped as an executable and will run everywhere.
- [Largo/cosmoruby: Actually Portable Ruby Executables](https://github.com/Largo/cosmoruby)
- [Largo/HacketyHack: HacketyHack working in 2026](https://github.com/Largo/hacketyhack). Includes [clogs](https://rubygems.org/gems/clogs), which runs Shoes programs on plain CRuby with native widgets (libui), no browser engine or JVM needed.
- I got Ruboto: Ruby on Android to work with JRuby 10 [largo/ruboto](https://github.com/Largo/ruboto). This means Ruby apps working on Android phones and WearOS.
- Working on Drivers and PHP internals.
- My friend Yosei Ito made a tool [prremote](https://github.com/lumbermill/prremote) to run mruby on Rasberry pi pico and ESP32s, which runs on the cmd line. It works very well with AI, because of it. I made [inkmodoro](https://github.com/Largo/inkmodoro) as a demo for it.
- Faster `require` on Windows, and YJIT on Windows, see [above](#a-small-part-in-making-ruby-faster-on-windows).
- [Historical Swiss Weather: see high/low temperatures on a map](https://github.com/Largo/swisshistoricalweather)

  <a href="https://github.com/Largo/swisshistoricalweather"><img src="https://raw.githubusercontent.com/Largo/swisshistoricalweather/main/docs/preview.png" alt="Map of daily maximum temperatures at 149 Swiss weather stations" width="600"></a>
- [Ruby.wasm Template](https://github.com/Largo/rubyWasmTemplate)
This is a template to start using Ruby.wasm in the browser.

- [Largo/ruby.wasm-quickstart: Your ruby scripts in the browser](https://github.com/Largo/ruby.wasm-quickstart)
- [Code for koans.idogawa.com](https://github.com/Largo/BrowserRubyKoans)

- [Largo/glimmer-dsl-web-standalone-demo: This is a demo of Glimmer DSL for Web without rails. It allows you to use Ruby instead of JavaScript.](https://github.com/Largo/glimmer-dsl-web-standalone-demo)

- 🔭 I’m currently working on improving Ruby for Windows. Checkout my ocra fork: [ocran](https://github.com/Largo/ocran).
- 👯 I’m looking to collaborate on Ruby! I went to Ruby Kaigi 2024, Ruby World 2024 and Ruby Kaigi 2026. Ruby now works anywhere: Windows, OSX, Linux, Android and the browser! Let's make it even easier for everyone to use it! I'm looking forward to Ruby Kaigi 2027 in Miyazaki, Japan, which I recommended to the organizers.
- 🤔 Ruby on Windows can still get faster. Ideas are welcome!
- 💬 Ask me about anything Ruby! See my [Ruby Gems](https://rubygems.org/profiles/largo)
- 📫 How to reach me: see my email on my website!
- Talk to me in English, (Swiss-)German, Japanese or French!
- Favorite Editor: Micro and VSCode.
- DevOps: I've set up Devcontainers for VSCode so developers don't have to setup their own development environment. I self-host Gitlab.
- ⚡ Fun fact: My approach to development is inspired by Derek Sivers. [Check out this cool podcast](https://remoteruby.com/216)

Fields of interest: Ruby, MRuby / Picoruby, PHP, Python, JavaScript/Typescript, C/C++/C#, SaaS, VSCode Devcontainers, Docker, Self-hosting, crossplattform (Linux, Windows, MacOS), ecommerce, 
single developer projects, talking to people and helping them using code, 
simple solutions like htmx

## Blogposts:
- [How to compile ruby on Windows and debug with GDB](https://idogawa.dev/p/2023/01/compile-ruby-windows.html)
- [How to programm in Ruby on rails your phone or android tablet](https://idogawa.dev/p/2022/11/how-to-code-in-ruby-on-rails-on-android-phone.html)
- [Edit your website on your android phone using termux and git!](https://idogawa.dev/p/2022/11/edit-website-on-android-phone.html)
- [Taking Website Screenshots with Ruby](https://idogawa.dev/p/2022/06/taking-website-screenshots-with-ruby.html)
- [How to git diff epub, docx and sqlite files](https://idogawa.dev/p/2024/01/git-diff-epub.html)
- [Ruby on the frontend demo app](https://idogawa.dev/p/2024/01/glimmer-dsl-web-demo.html)
- [Exploring the Dynamic World of Animated SVG Favicons](https://idogawa.dev/p/2024/01/svg-emoji-favicons.html)
