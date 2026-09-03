# IGCSE Question Bank

A revision paper generator I built for my own tutoring practice and use every week. This repository holds the design notes. It contains no exam content: the questions and mark schemes are third-party material and stay in a private repository.

## The problem

I teach IGCSE students from revision notes, then send them past paper questions on the same topic. The questions do not respect chapter boundaries. A set notation question assumes three-circle Venn diagrams that the notes never draw. A prime factors question needs an index law from a later chapter. The student stalls on something I have not taught yet, and concludes they are bad at the topic when they are not.

Sorting that by hand does not scale past a few topics.

## What it does

It takes a topic and a set of revision notes and produces two documents. A student paper with only the questions answerable from what has been taught, and a teacher paper with everything, including the removed questions, the mark schemes and the reasoning behind each removal.

Current state: 2256 questions across IGCSE Maths Extended, Physics and Chemistry. 2143 sent, 113 removed, 160 flagged, 7545 marks, 148 generated PDFs.

## The scope rule

A question is removed only when answering it requires a technique taught in a different chapter. General numeracy the notes assume, such as place value or reading a list, is kept.

Maths is judged mechanically. Every chapter of the course has a position in a fixed order, and a function compares the position of the chapter a question sits in against every chapter it reaches into. A chapter absent from the list counts as never taught.

Physics and chemistry are judged by hand against the syllabus, because those courses are taught in sequence: every earlier chapter counts as covered, and only later chapters remove anything. Chemistry 3.2 would be impossible otherwise. Its notes never balance an equation, they only use balanced ones, and balancing is the chapter immediately before.

A third state sits between kept and removed. A question that stays in the paper but uses something the notes never demonstrate is flagged, so I know to say a word before setting it.

Answer papers keep every question, including removed ones. Removed questions carry no number, so numbering matches the paper students received and a marked script lines up.

## Build pipeline

Python, Node and headless Chromium, in four steps.

Diagram preparation produces three sizes from the masters: 850px for question papers that get printed and written on, 620px for worked answers read on screen, 400px thumbnails inlined into the site. All quantised to 16 colours, which is visually lossless for line art. Faint diagrams are contrast-stretched, because many source Venn diagrams and factor trees are grey hairlines that barely survive printing.

Maths typesetting runs KaTeX ahead of time into static HTML, with font faces inlined, so nothing loads a maths library at runtime.

Paper rendering builds each paper as HTML and prints it to PDF through headless Chromium.

Site building writes an index for each destination.

## Two audiences, two builds

The student site is not the teacher site with answers hidden in the interface. Working, answers, marking, scope reasoning and difficulty tiers are stripped from the data before the student site is written, because otherwise view-source hands over every mark scheme.

## Things that went wrong

Kept so they do not happen twice. A few of the more instructive ones.

A missing KaTeX font fails silently. A script capital E rendered as a plain italic E for an entire build.

The PDF text layer is corrupt for mathematics. It turns a square root of 7 into 7, mangles superscripts and drops whole answers, so questions are transcribed from page images instead.

Comparing channel means is not a greyscale test. A mostly-white page with a few red answer marks has near-identical channel means, and flattening it destroys the annotations that identify it as an answer diagram. The fix counts pixels off the grey axis.

Topic order used to depend on which merge script ran last, so re-running one moved its topic to the end and Fractions printed after Percentages despite being taught before it. Order now lives in one file and every merge sorts by it.

A literal dollar sign is a maths delimiter. Currency in a question stem has to be written as an HTML entity or it closes the span early.

## Source errors

The source PDFs contain genuine mistakes. Where one is unambiguous it is corrected and recorded, then printed in the answer paper under a note saying whether it was corrected or left as printed. 194 so far. One example: a mark scheme defined the complement of an intersection as the elements in both sets, a dropped negation reversing the definition.

## Contact

Agga Thu, Bangkok. agga.suzichao@gmail.com
