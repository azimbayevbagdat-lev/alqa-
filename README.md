# Алқа: Claude Code-тағы бизнес кеңесі

Бизнес сұрағыңызды жазасыз, ал Claude Code жеті кеңесшіні жинайды: Джефф Безос, Уоррен Баффет,
Алекс Хормози, Гэри Вайнерчук, Пол Грэм, Дэн Мартелл, Джино Уикман. Әрқайсысы жеке агент: алдымен
өз пікірін айтады, сосын бір-бірімен дауласады, соңында әр нұсқаға 1-ден 10-ға дейін ұпай береді.
Нәтижесі: шешімі, ұпайлар кестесі, кім келіспегені және келесі қадамдар жазылған есеп (Markdown және PDF).

> Кеңесшілер нағыз адамдар емес. Бұл олардың ашық кітаптары мен сөйлеген сөздерінен жасалған AI
> еліктеу. Оны ойлануға көмекші құрал ретінде қолданыңыз, кәсіби кеңес деп қабылдамаңыз.

## Орнату

1. Zip файлды ашыңыз. Ішінде `.claude` папкасы бар, оның ішінде `skills` және `agents` папкалары.
   (macOS-та нүктемен басталатын папкалар жасырын: Finder-де Cmd+Shift+. басыңыз.)
2. Екі жолдың бірін таңдаңыз:
   - **Бір жобаға**: `.claude` папкасын жоба папкасына көшіріңіз. Жобада `.claude` бұрыннан бар
     болса, `skills` пен `agents` папкаларының ішіндегісін соған қосыңыз.
   - **Барлық жобаларға**: `skills` пен `agents` папкаларының ішіндегісін үйдегі `.claude`
     папкасына көшіріңіз: Windows-та `C:\Users\<атыңыз>\.claude\`, macOS пен Linux-та `~/.claude/`.
3. Claude Code-ты жауып, қайта ашыңыз.

## Қолдану

Claude Code-қа жазыңыз:

```
алқаны жина: Алматыда кофехана ұстаймын, айына 4 000 кесе сатамын, бір кесе 1 200 теңге.
Бағаны 1 500 теңгеге көтерейін бе?
```

немесе `/alqa <сұрағыңыз>`. Сандарды көбірек жазсаңыз, кеңес соғұрлым нақты болады.
Есеп жоба папкасындағы `alqa-reports` папкасына сақталады.

Өз кеңесшіңізді қосу: `.claude/agents/` ішіндегі бір файлды көшіріп, атын `alqa-<есім>.md` деп
өзгертіңіз, ішіндегі `name` жолын сол атқа ауыстырып, тұлғаның қағидаларын жазыңыз. Аты `alqa-`
деп басталатын әр агент кеңеске өзі қосылады.

---

# Alqa: a business council inside Claude Code

Ask a business question and Claude Code convenes seven advisor subagents (Jeff Bezos, Warren Buffett,
Alex Hormozi, Gary Vaynerchuk, Paul Graham, Dan Martell, Gino Wickman). Each gives an opening view,
then they argue with each other by name, then each scores every option 1-10 with a one-line reason.
You get a report with the decision, the score table, who dissented and next steps, as Markdown and PDF.

> The advisors are AI imitations based on public books, talks and interviews, not the real people,
> who have not reviewed or endorsed them. Use it as a thinking aid, not as professional advice.

## Install

1. Unzip. Inside is a `.claude` folder with `skills/alqa/SKILL.md` and `agents/alqa-*.md`.
2. Either copy `.claude` into a project folder (merge with an existing `.claude`), or copy the
   contents of `skills` and `agents` into `~/.claude/skills/` and `~/.claude/agents/` to use it in
   every project.
3. Restart Claude Code.

## Use

Ask `алқаны жина: <question>`, "convene the council: <question>", or `/alqa <question>`.
Reports go to `alqa-reports/` in the current project. The PDF is made with what your computer
already has (a Chromium browser in headless mode, or pandoc); Claude asks before installing anything.

Add an advisor: copy any `alqa-*.md` file, rename it, change `name` in the front matter and write
the persona. Every subagent whose name starts with `alqa-` joins the council.

Note: a full council is 14 subagent runs, so it uses more of your plan's limits than a normal
question. Ask for fewer advisors ("only Hormozi, Buffett and Graham") for quick questions.
