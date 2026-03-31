# Toffu Marketing Skills

A collection of marketing skills that give your AI agent persistent background knowledge. These skills are used in [Toffu](https://toffu.ai) and are open source - we welcome contributions!

## What are Skills?

Skills are **persistent knowledge** that your AI agent keeps in mind during every conversation. Unlike [playbooks](https://github.com/toffu-ai/playbooks) (step-by-step workflows you run on demand), skills shape how your agent thinks and responds all the time.

Examples:
- **Brand Voice** — tone, vocabulary, and writing style rules
- **Target Audience** — who you're selling to, their pain points, and exclusions
- **Channel Constraints** — which platforms and formats to use or avoid

## Structure

Each skill is a markdown file with YAML frontmatter containing metadata. Here's an example:

```markdown
---
id: brand-voice
title: Brand Voice Guidelines
description: Defines tone, vocabulary, and writing style for all content
tags:
  - brand
  - content
  - writing
category: brand
author:
  name: Jane Smith
  pic_url: /jane.jpeg
  profile_url: https://linkedin.com/in/jane-smith
---

Write in a conversational, direct tone. Avoid corporate jargon...
```

## Contributing

1. Fork this repository
2. Create a new markdown file in the `skills/` directory
3. Follow the frontmatter structure above
4. Make sure your skill includes:
   - Clear, specific guidance the agent can follow
   - Concrete examples where helpful (do this / don't do this)
   - Appropriate tags and category
   - Your author information
5. Submit a pull request

### Skill Guidelines

- Keep guidance specific and actionable — avoid vague advice
- Use examples to show what good and bad output looks like
- Focus on one area of knowledge per skill
- Write as instructions to the AI agent, not documentation for humans
- Include constraints and boundaries, not just positive guidance

## License

MIT License - feel free to use these skills in your own projects!
