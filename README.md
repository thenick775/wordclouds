Wordclouds in go.

![alt text](example/output.png "Example")

# How to use

```go
wordCounts := map[string]int{"important":42, "noteworthy":30, "meh":3}

var cols = []color.RGBA{
	{0x1b, 0x1b, 0x1b, 0xff},
	{0x48, 0x48, 0x4B, 0xff},
	{0x59, 0x3a, 0xee, 0xff},
	{0x65, 0xCD, 0xFA, 0xff},
	{0xff, 0xff, 0xff, 0xff},
	{0x70, 0xD6, 0xBF, 0xff},
}

colors := make([]color.Color, 0)
for _, c := range cols {
	colors = append(colors, c)
}

w := wordclouds.NewWordcloud(
        wordCounts,
	wordclouds.FontFile("fonts/myfont.ttf),
	wordclouds.FontMaxSize(600),
	wordclouds.FontMinSize(15),
	wordclouds.Colors(colors), //array of color.Color
	wordclouds.BackgroundColor(color.RGBA{255, 255, 255, 255}), //optional background color.Color
	wordclouds.Height(2048),
	wordclouds.Width(2048),
	wordclouds.RandomPlacement(false)
)

img := w.Draw()
```

# Options

- Output height and width
- Font: Must be a valid TTF file.
- Font max,min size
- Colors
- Background color
- Placement : random or circular
- Masking

# Masking

A list of bounding boxes where the algorithm can not place words can be provided.

The `Mask` function can be used to create such a mask given a file and a masking color.

```go
boxes := wordclouds.Mask(
	conf.Mask.File,
	conf.Width,
	conf.Height,
	conf.Mask.Color,
)
```

See the example folder for a fully working implementation.

# Speed

Most wordclouds should take a few seconds to be generated. A spatial hashmap is used to find potential collisions.

There are two possible placement algorithm choices:
1. Random: the algorithms randomly tries to place the word anywhere in the image space.
   - If it can't find a spot after 500000 tries, it gives up and moves on to the next word. It's quite slow.
2. Spiral: the algorithm starts to place the words on concentric circles starting at the center of the image.
It is very fast and is the default algorithm    

# Contributing

Feel free to create pull requests, I'll gladly review them!
