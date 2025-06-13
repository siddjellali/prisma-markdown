# Implicit M: N relationships

> Generated by [`prisma-markdown`](https://github.com/samchon/prisma-markdown)

- [Articles](#articles)

## Articles

```mermaid
erDiagram
"bbs_articles" {
  String id PK
  DateTime created_at
  DateTime deleted_at "nullable"
}
"bbs_article_snapshots" {
  String id PK
  String bbs_article_id FK
  String format
  String title
  String body
  DateTime created_at
}
"attachment_files" {
  String id PK
  String name "nullable"
  String extension "nullable"
  String url
  DateTime created_at
}
"bbs_article_comments" {
  String id PK
  String bbs_article_id FK
  String parent_id FK "nullable"
  DateTime created_at
  DateTime deleted_at "nullable"
}
"bbs_article_comment_snapshots" {
  String id PK
  String bbs_article_comment_id FK
  String format
  String body
  DateTime created_at
}
"_attachment_filesTobbs_article_snapshots" {
  String A FK
  String B FK
}
"_attachment_filesTobbs_article_comment_snapshots" {
  String A FK
  String B FK
}
"bbs_article_snapshots" }|--|| "bbs_articles" : article
"bbs_article_comments" }o--|| "bbs_articles" : article
"bbs_article_comments" }o--o| "bbs_article_comments" : parent
"bbs_article_comment_snapshots" }|--|| "bbs_article_comments" : comment
"_attachment_filesTobbs_article_snapshots" }o--|| "attachment_files" : attachment_files
"_attachment_filesTobbs_article_snapshots" }o--|| "bbs_article_snapshots" : bbs_article_snapshots
"_attachment_filesTobbs_article_comment_snapshots" }o--|| "attachment_files" : attachment_files
"_attachment_filesTobbs_article_comment_snapshots" }o--|| "bbs_article_comment_snapshots" : bbs_article_comment_snapshots
```

### `bbs_articles`

Article entity.

`bbs_articles` is a super-type entity of all kinds of articles in the
current backend system, literally shaping individual articles of
the bulletin board.

And, as you can see, the elements that must inevitably exist in the
article, such as the title or the body, do not exist in the `bbs_articles`,
but exist in the subsidiary entity, [bbs_article_snapshots](#bbs_article_snapshots), as a
1: N relationship, which is because a new snapshot record is published
every time the article is modified.

The reason why a new snapshot record is published every time the article
is modified is to preserve the evidence. Due to the nature of e-community,
there is always a threat of dispute among the participants. And it can
happen that disputes arise through articles or comments, and to prevent
such things as modifying existing articles to manipulate the situation,
the article is designed in this structure.

In other words, to keep evidence, and prevent fraud.

Properties as follows:

- `id`:
- `created_at`: Creation time of article.
- `deleted_at`
  > Deletion time of article.
  >
  > To keep evidence, do not delete the article, but just mark it as
  > deleted.

### `bbs_article_snapshots`

Snapshot of article.

`bbs_article_snapshots` is a snapshot entity that contains the contents of
the article, as mentioned in [bbs_articles](#bbs_articles), the contents of the
article are separated from the article record to keep evidence and prevent
fraud.

Properties as follows:

- `id`:
- `bbs_article_id`: Belong article's [bbs_articles.id](#bbs_articles)
- `format`
  > Format of body.
  >
  > Same meaning with extension like `html`, `md`, `txt`.
- `title`: Title of article.
- `body`: Content body of article.
- `created_at`
  > Creation time of record.
  >
  > It means creation time or update time or article.

### `attachment_files`

Attachment File.

Every attachment files that are managed in current system.

For reference, it is possible to omit one of file name or extension like
`.gitignore` or `README` case, but not possible to omit both of them.

Properties as follows:

- `id`:
- `name`
  > File name, except extension.
  >
  > Possible to omit like `.gitignore` case.
- `extension`
  > Extension.
  >
  > Possible to omit like `README` case.
- `url`: URL path of the real file.
- `created_at`: Creation time of file.

### `bbs_article_comments`

Comment written on an article.

`bbs_article_comments` is an entity that shapes the comments written on an
article.

And for this comment, as in the previous relationship between
[bbs_articles](#bbs_articles) and [bbs_article_snapshots](#bbs_article_snapshots), the content body
of the comment is stored in the sub [bbs_article_comment_snapshots](#bbs_article_comment_snapshots)
table for evidentialism, and a new snapshot record is issued every time
the comment is modified.

Also, `bbs_article_comments` is expressing the relationship of the
hierarchical reply structure through the `parent_id` attribute.

Properties as follows:

- `id`:
- `bbs_article_id`: Belonged article's [bbs_articles.id](#bbs_articles)
- `parent_id`
  > Parent comment's [bbs_article_comments.id](#bbs_article_comments)
  >
  > Used to express the hierarchical reply structure.
- `created_at`: Creation time of comment.
- `deleted_at`
  > Deletion time of comment.
  >
  > Do not allow to delete the comment, but just mark it as deleted,
  > to keep evidence.

### `bbs_article_comment_snapshots`

Snapshot of comment.

`bbs_article_comment_snapshots` is a snapshot entity that contains the
contents of the comment.

As mentioned in [bbs_article_comments](#bbs_article_comments), designed to keep evidence
and prevent fraud.

Properties as follows:

- `id`:
- `bbs_article_comment_id`: Belonged article's [bbs_article_comments.id](#bbs_article_comments)
- `format`
  > Format of content body.
  >
  > Same meaning with extension like `html`, `md`, `txt`.
- `body`: Content body of comment.
- `created_at`
  > Creation time of record.
  >
  > It means creation time or update time or comment.

### `_attachment_filesTobbs_article_snapshots`

Pair relationship table between [attachment_files](#attachment_files) and [bbs_article_snapshots](#bbs_article_snapshots)

Properties as follows:

- `A`:
- `B`:

### `_attachment_filesTobbs_article_comment_snapshots`

Pair relationship table between [attachment_files](#attachment_files) and [bbs_article_comment_snapshots](#bbs_article_comment_snapshots)

Properties as follows:

- `A`:
- `B`:
