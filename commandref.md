# S3 High-Level Commands

This guide provides you with a high-level overview of the S3 commands.

## Path Arguments

A path argument specifies the URL of a resource. It can either of **LocalPath** or **S3Uri** type; at least one of these must be specified.

### LocalPath

LocalPath points to a local file or directory. It can be either an absolute or a relative path.

### S3Uri

S3Uri can either specify the location of an S3 object, prefix, or bucket; or an S3 access point.

An S3Uri must start with *s3://*. Prefixes are separated by forward slashes. 

#### Specifying a location

Use the `s3://mybucket/mykey` syntax to specify the location of a resource. 

**Example:** 

In the `s3://mybucket/myprefix/myobject` S3Uri: 

- *myobject* is the S3 object
- *myprefix* is the prefix
- *myprefix/myobject* is the S3 key
- *mybucket* is the S3 bucket


#### Specifying an access point

S3Uri can also specify S3 access points in `s3://<access-point-arn>/<key>` format. 

**Example:** 

In the `s3://arn:aws:s3:us-west-2:123456789012:accesspoint/myaccesspoint/mykey` S3Uri:

- *myaccesspoint* is the access point 
- *arn:aws:s3:us-west-2:123456789012:accesspoint/myaccesspoint* is the ARN
- *mykey* is the key


**NOTE:** Similar to bucket names, you can also use prefixes with access point ARNs for the S3Uri. For example: `s3://arn:aws:s3:us-west-2:123456789012:accesspoint/myaccesspoint/myprefix/`

## Order of Path Arguments

Commands can have up to two positional path arguments:

- The first path argument represents the source, which is the local file/directory or S3 object/prefix/bucket that is being referenced. 
- The second path argument, if present, specifies the destination, which is the local file/directory or S3 object/prefix/bucket that is being operated on. 
 
Commands with only one path argument perform the operation only on the source.

## Single Local File and S3 Object Operations

The following commands perform operations only on a single file or S3 object by default. Use the `--recursive` flag to run these commands recursively.

- **copy:** `cp <source> <target>`
- **move:** `mv <source> <target>`
- **remove:** `rm <source> <target>` 
  
`<source>` must be the first path argument. It can point to a local file or S3 object. 

`<target>` is the second path argument. It can be a local file, local directory, S3 object, S3 prefix, or S3 bucket. If it indicates a local directory, S3 prefix, or S3 bucket, it can end with a forward slash or back slash. If the destination ends with a slash, the destination file or object adopts the name of the source file or object. Otherwise, the file or object is saved under the name provided.

**NOTE:** The path argument type determines whether to use forward slash or backward slash as a separator:

- Separators in **LocalPath** arguments should match the preference of the operating system.
- In an **S3Uri**, forward slash must be used.

## Directory and S3 Prefix Operations

The following commands always perform operations on the contents of a local directory or S3 prefix/bucket, regardless of whether the path argument ends with a slash or not:

- **synchronize:** `sync <source> <target>`
- **make bucket:** `mb <target>`
- **remove bucket:** `rb <target>`
- **list content:** `ls <target>`

## Using Filters

Most commands have `--exclude "<value>"` and `--include "<value>"` parameters that either exclude or include a particular file or object.

The following patterns and symbols are supported.

| Pattern/symbol | Description |
| ----------- | ----------- |
| `*` | Matches everything |
| `?` | Matches any single character |
| `[sequence]` | Matches any character in sequence |
| `[!sequence]` | Matches any character not in sequence |

**NOTE:** UNIX-style wildcards in a command's path arguments are currently not supported.

A command can contain multiple filters. To do this, use the `--exclude` or `--include` argument multiple times. For example: `--include "*.txt" --include "*.png"`

In the case of multiple filters, filters that appear later in the command take precedence over filters that appear earlier. For example, `--exclude "*" --include "*.txt"` means that all files are excluded from the command except for .txt files. However, `--include "*.txt" --exclude "*"` excludes all files from the command.

By default, all files are included, therefore, providing only an `--include` filter does not change anything. The `--include` flag applies only to files already excluded with an `--exclude` filter. For example, if you only want to upload files with a particular extension, you need to first exclude all files, then re-include the files with the particular extension. 

Filters are always evaluated against the source **directory**. If the source is a file instead of a directory, then the directory containing the file is considered the source directory. 

### Examples

The following examples demonstrate the behavior of commands with the following source directory.  


```
/tmp/foo/
  .git/
  |---config
  |---description
  foo.txt
  bar.txt
  baz.jpg
```


```
aws s3 sync /tmp/foo s3://bucket/
```
This command synchronizes the /tmp/foo directory with the s3://bucket/.


```
aws s3 cp /tmp/foo s3://bucket/ --recursive --exclude ".git/*"
```
This command copies the content of /tmp/foo to the s3://bucket/, except for the content of the .git subfolder (.git/config and .git/description).


```
aws s3 cp /tmp/foo/ s3://bucket/ --recursive --exclude "ba*"
```
This command copies the content of /tmp/foo to the s3://bucket/, except for bar.txt and baz.jpg.


```
aws s3 cp /tmp/foo/ s3://bucket/ --recursive --exclude "*" --include "*.jpg"
```
This command upload only .jpg files.


```
aws s3 cp /tmp/foo/ s3://bucket/ --recursive --exclude "*" --include "*.jpg" --include "*.txt
```
This command uploads .jpg and .txt files.
