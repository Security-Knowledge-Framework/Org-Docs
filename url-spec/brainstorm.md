Hey :wave:, @Jonathan Leitschuh I was working on the proposals for unifying the URL specs. From the research I’ve done on the topic so far, we’d need a total of 2 RFC's to solve to problem and ensure it never occurs again.

****Unified URI RFC** (The 1st RFC)**

The 1st RFC (**The **Unified URI RFC****) will use the original [2005 URI RFC](https://www.rfc-editor.org/rfc/rfc3986) as it’s basis, then build on top of it the modern [WHATWG URL standard](https://github.com/whatwg/url). [Alwin Blok](https://github.com/alwinb) already created a [new URL Spec. proposal](https://alwinb.github.io/url-specification/) to try to accomplish this very goal, we’ll investigate what changes there are between both RFC’s and the new proposal by [Alwin](https://github.com/alwinb), and look to create a **Unified RFC from** that.

Some ideas that @Randall, [Jonathan Neal](https://twitter.com/jon_neal), and I played with were separating URI’s and URL’s.
* URI’s represent any form a path locator can take, e.g. `C://windows/path/to/file`, `/root/mnt/this/goes/somewhere`, `file:///the/file/system/access`, or `https://google.com`. The key take away is that there is no limitations to the nature of the file path, so long as it functions as a path indicator. We’d encourage websites, apps and browsers to start using URI’s instead of URL’s as URL’s in this new spec. will be a very specific and limited form of URI’s.
* URL’s will be URI’s but with a stricter set of rules, with most of those being inherited from the [WHATWG URL standard](https://github.com/whatwg/url), making sure to make changes when it conflicts with the original [2005 URI RFC](https://www.rfc-editor.org/rfc/rfc3986), and make additional changes based off of [Alwin Blok](https://github.com/alwinb)’s new [URL Spec. proposal](https://alwinb.github.io/url-specification/). We’d also look into other complaints about the current URL Spec. including those from the [cURL team](https://daniel.haxx.se/blog/2017/01/30/one-url-standard-please/). Ideally we’d also want to also look into current and future use cases and try to cover using URL’s as a state storage mechanism (one could argue you shouldn’t use URL’s to store state, but devs do, since it’s quick and easy, we should look into making recommendations for how devs should handle storing state in URL’s). 

> The reason for making URI’s and URL’s different is because URL’s in the minds of devs represents both `./paths/like/this` and `https://a.path.like/this`, but the spec as it’s currently written really only works for URL’s, which means you run into issues in `Node.js` like environments where local and relative paths are supported, but to which the URL syntax does not support. 

**Goals**:
* Backward compatible
* Realistic (we’re not going to be able to change the entire web, our goal is to unify use cases)
* As close to complete agreement on the spec as possible
* Buy in from all major vendors, e.g. browsers, app devs, os devs, library devs, major languages, and legacy application use cases and users
* Trialed e.g. each vendor will gradually test out the new URI spec. looking for any broken links or potentially missed use cases
* Security as a 1st principle. 
  * We should make recommendations on how to handle scams, phishing, and more… since we’re being as backward compatible as possible it might leave some security holes, that might cause problems for users.
  * For example, (when a user users a language with the latin character set) recommending browsers and other applications, highlight characters that are not the basic latin characters and use a different font for those specific characters to make it clear that said URL might be leading users to the wrong site, etc… (More research necessary to ensure this solution is able to work for CLI tools, libraries and other applications)


:thread: /1


/2 **URI Versioning (RFC 2)**

This RFC focuses on future-proofing the **Unified URI spec**. mentioned in the previous RFC, it focuses on the question "What happens if some usecase we never imagined appears and the current **Unified URI spec**. isn't enough?"

The ideal would be that the **Unified URI spec**. is enough but with AI, Quantum Computing and other future technologies on the horizon, we cannot predict the needs of the future, so we need to prepare a mechanism to version the URI standard supported by applications. The idea behind versioning is to support a what I'd like to call a `version:` brand, the brand will only represent versions that are greater than the version 0 which is the accepted **Unified URI spec**., in simpler terms, by default a URI won't need a `version:` brand, we'll just assume that by not having a `version:` brand the **Unified URI spec**. is what is meant. 

> **Remember**: The **Unified URI spec**. is backward compatible, so we get to have our cake and have it too


A couple question surrounding this RFC appear:
* Semantic Versioning? 
  * **Ans**: YES! All changes must be desperately needed before it should be added and ideally and new version should wait 2 years before being applied, giving more than enough time for it to propagate through the system
* What is the version brand?
  * **Ans**: It’s not been determined yet, and doesn’t need to be determined yet. Ideally what we need to determine is the separator that identifies the version: brand and the URI 
* How would this version brand function across language styles? e.g. left-to-right vs right-to-left languages
  * **Ans**: It would probs be always the first set of characters read, to which it then switches to the URI after encountering the separator
* What will be the separator?
  * **Ans**: Currently I’m leaning toward : as it’s already recognizable and accepted as a separator for a URL schema and it’s domain

The real questions surrounding this RFC is what do we want applications to do if they do not support the specified version brand?

Potential options: 
* Throw a `URI Version Brand Not Supported Error`
* Build in a mechanism that automatically fetches new `URI Version Brand Parsing Rules` :face_exhaling: 
  * ^ This requires looking into a syntax for parsing syntax rules, potentially worth while as a seperate larger project
* Use the DNS Resolver System 
* Let each application choose one of the options specified

**Goals:**
* Backward compatible
* Realistic
* As close to complete agreement on the spec as possible
* Buy in from all major vendors, e.g. browsers, app devs, os devs, library devs, major languages, and legacy application use cases and users
* Trialed e.g. each vendor will test it out to see how plausible it would be
* Security as a 1st principle. 
  * We should make recommendations on how to handle scams, phishing, and more… since we’re being as backward compatible as possible it might leave some security holes, that might cause problems for users.
  * For example, (when a user users a language with the latin character set) recommending browsers and other applications, highlight characters that are not the basic latin characters and use a different font for those specific characters to make it clear that said URL might be leading users to the wrong site, etc… (More research necessary to ensure this solution is able to work for CLI tools, libraries and other applications)

/2 - End

> **Note**: All recommendations (security, and not) will be documented as Living Standards, allowing us to make changes as new security vulnerabilities appear