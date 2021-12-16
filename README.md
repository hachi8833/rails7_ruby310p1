## Requirement

- Docker Desktop 4.2.0 (70708) -- in the case of macOS
  - Docker 20.10.10, build b485636
  - Compose 1.29.2
- [dip](https://github.com/bibendi/dip)
  - You need a Ruby environment in your host computer: use rbenv or like that

## Usage

- `dip build`
- `dip bundle install`

Then you can whatever you like on Rails 7.0.0 and Ruby 3.1.0-preview1 with YJIT activated, such as:

- `dip rails new . -a propshaft --skip-git` or - `dip rails new . --css bootstrap --skip-git`
- `dip rails s` to start Rails