# Large-scale Database — Notes

LaTeX notes for the Large-scale Database course (University of Pisa), written lesson by lesson.

The compiled notes are in [`main.pdf`](main.pdf).

## Structure

- `main.tex`: preamble, title page, table of contents and one `\input` per lesson.
- `lessons/lessonNN.tex`: the content of each lesson.
- `labs/`: material from the lab sessions.
- `main.pdf`: the compiled notes.

The course material (slides) is kept in `docs/` and is not part of the repository.

## Build

```sh
latexmk -pdf main.tex
```
