Ivy [![godoc badge](http://godoc.org/github.com/plimble/ivy?status.png)](http://godoc.org/github.com/plimble/ivy)   [![gocover badge](http://gocover.io/_badge/github.com/plimble/ivy?t=5)](http://gocover.io/github.com/plimble/ivy) [![Build Status](https://api.travis-ci.org/plimble/ivy.svg?branch=master&t=5)](https://travis-ci.org/plimble/ivy) [![Go Report Card](http://goreportcard.com/badge/plimble/ivy?t=5)](http:/goreportcard.com/report/plimble/ivy)
=========

Assets & Image processing on the fly by GraphicsMagick

### Installation
`go get -u github.com/plimble/ace`

#####GraphicsMagick

Ubuntu
```shell
sudo apt-get install python-software-properties
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:rwky/graphicsmagick
sudo apt-get update
sudo apt-get install graphicsmagick
```

OSX
```
brew install graphicsmagick
```

### Documentation
 - [GoDoc](http://godoc.org/github.com/plimble/ivy)

### Sources

##### File System

```
	source := ivy.NewFileSystemSource("/path/to/asset")
	cache := ivy.NewFileSystemCache("/path/to/cache")
	processor := ivy.NewGMProcessor()

	config := &ivy.Config{
		IsDevelopment: false,
		HttpCache:     66000,
	}

	iv := ivy.New(source, cache, processor, config)
```

##### AWS S3

```
	source := ivy.NewS3Source("accessKey", "secretKey")
	cache := ivy.NewFileSystemCache("/path/to/cache")
	processor := ivy.NewGMProcessor()

	config := &ivy.Config{
		IsDevelopment: false,
		HttpCache:     66000,
	}

	iv := ivy.New(source, cache, processor, config)
```

### Cache

You can use file system for caching or set `nil` for CDN like Cloudfront or disable caching


### Server

You can run built-in server or implement in your server

For more config

```
./server -h
```

##### Ace Example

```
	a := ace.New()
	a.GET("/:bucket/:params/*path", func(c *ace.C) {
		iv.Get(
			c.Params.ByName("bucket"),
			c.Params.ByName("params"),
			c.Params.ByName("path"),
			c.Writer,
			c.Request,
		)
	})

	a.Run(":3000")
```

You can run as middleware also

### Example Request Image
Get image with original size or non image

```
	http://localhost:3000/bucket/_/test.jpg
```

```
	http://localhost:3000/bucket/0/test.jpg
```

Resize 100x100

```
	http://localhost:3000/bucket/r_100x100/test.jpg
```

Resize width 100px aspect ratio

```
	http://localhost:3000/bucket/r_100x0/test.jpg
```

Crop top left image 200x200

```
	http://localhost:3000/bucket/c_200x200/test.jpg
```

Crop with gravity NorthWest image 200x200

```
	http://localhost:3000/bucket/c_200x200,g_nw/test.jpg
```

Resize 400x400 then crop 200x200 and gravity center

```
	http://localhost:3000/bucket/r_400x400,c_200x200,g_c/test.jpg
```

Quality 100

```
	http://localhost:3000/bucket/q_100/test.jpg
```

###Params Table

| Param               | Description                            |
|---------------------|----------------------------------------|
| r_{width}x{height}  | Resize image, if 0 is aspect ratio     |
| c_{width}x{height}  | Crop image                             |
| g_{direction}       | Gravity image                          |
| q_{quality}         | Quality image maximum 100              |

###Gravity position

| Param | Description                            |
|-------|----------------------------------------|
| nw    | North West                             |
| n     | North                                  |
| ne    | North East                             |
| w     | West                                   |
| c     | Center                                 |
| e     | East                                   |
| sw    | South West                             |
| s     | South                                  |
| se    | South East                             |


###Contributing

If you'd like to help out with the project. You can put up a Pull Request.

