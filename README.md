## ProgressBar: A Text Progress Bar Library for Ruby
[![Build Status](https://travis-ci.org/market76/progressbar.png?branch=master)](https://travis-ci.org/market76/progressbar)

Ruby/ProgressBar is a text progress bar library for Ruby.
It can indicate progress with percentage, a progress bar,
and estimated remaining time.

### Usage

In your Gemfile, add the following line:

```ruby
gem 'progressbar', git: 'https://github.com/realtimeMD/progressbar.git', branch: 'master'
```

### Examples

```

  % irb --simple-prompt -r progressbar
  >> pbar = ProgressBar.new("test", 100)
  => (ProgressBar: 0/100)
  >> 100.times {sleep(0.1); pbar.inc}; pbar.finish
  test:          100% |........................................| Time: 00:00:10
  => nil

  >> pbar = ProgressBar.new("test", 100)
  => (ProgressBar: 0/100)
  >> (1..100).each{|x| sleep(0.1); pbar.set(x)}; pbar.finish
  test:           67% |..........................              | ETA:  00:00:03
  
  >> ProgressBar.new("test", 100) do |pbar|
  >>   100.times { sleep(0.1); pbar.inc }
  >> end
  test:          100% |........................................| Time: 00:00:10
  
```
  
### Usage
```ruby
ProgressBar#new (title, total, out = STDERR, &block)
```
    
Display the initial progress bar and return a
ProgressBar object.  ((|title|)) specifies the title,
and ((|total|)) specifies the total cost of processing.
Optional parameter ((|out|)) specifies the output IO.

The display of the progress bar is updated when one or
more percent is proceeded or one or more seconds are
elapsed from the previous display.

It also accepts a block in case you prefer not needing
to .finish the bar (see example above).

```ruby
ProgressBar#inc (step = 1)
```

Increase the internal counter by ((|step|)) and update
the display of the progress bar. Display the estimated
remaining time on the right side of the bar. The counter
does not go beyond the ((|total|)).

```ruby
ProgressBar#set (count)
```

Set the internal counter to ((|count|)) and update the
display of the progress bar. Display the estimated
remaining time on the right side of the bar.  Raise if
((|count|)) is a negative number or a number more than
the ((|total|)).


```ruby
ProgressBar#finish
```

Stop the progress bar and update the display of progress
bar. Display the elapsed time on the right side of the bar.
The progress bar always stops at 100 % by the method.

```ruby
ProgressBar#halt
```

Stop the progress bar and update the display of progress
bar. Display the elapsed time on the right side of the bar.
The progress bar stops at the current percentage by the method.

```ruby
ProgressBar#format=
```

Set the format for displaying a progress bar.
Default: "%-14s %3d%% %s %s".

```ruby
ProgressBar#format_arguments=
```

Set the methods for displaying a progress bar.
Default: [:title, :percentage, :bar, :stat].

```ruby
ProgressBar#file_transfer_mode
```

Use  :stat_for_file_transfer instead of :stat to display
transfered bytes and transfer rate.

```ruby
ProgressBar#long_running
```

Use adaptative running average to compute ETA, more effective for
long-running jobs or jobs whose speed may vary.


ReverseProgressBar class is also available.  The
functionality is identical to ProgressBar but the direction
of the progress bar is just opposite.

### Limitations

Since the progress is calculated by the proportion to the
total cost of processing, Ruby/ProgressBar cannot be used if
the total cost of processing is unknown in advance.
Moreover, the estimation of remaining time cannot be
accurately performed if the progress does not flow uniformly.
