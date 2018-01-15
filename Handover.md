# Understanding why you should start using grid and fallbacks

  CSS Grid is the future of how CSS layout will be done. At current time of writing support for it is at **75%** . Even this is a little off as **15% ** of this is made up of browsers OSM doesn't "actively" support (What i mean by this is we never test on them, i'm not sure if they are even covered in our browser support list but there probably would be an effort made to support them if requested).

  When Flex first came about, OSM started using that and had to write sometimes quite large fallback files to accomodate for this. Grid is nothing like this. Most Grid examples can be achieved in at most 7 lines or so of code.

  Take this footer as an example

  ![](https://i.imgur.com/6mPCKMN.png)

  ## Flex

      .ft-Footer_Columns {
       @include Grid_Row;
      }
      
      .ft-Footer_Column {
      	@include Grid_Column;
      }
      
      .ft-Footer_Column-content {
      	width: calc((5 / 12 * 100%) * (var(--Grid_Gutter) * 2));
      }
      
      .ft-Footer_Column-links {
      	width: calc((2 / 12 * 100%) * (var(--Grid_Gutter) * 2));
      }
      
      .ft-Footer_Column-contact {
      	width: calc((2 / 12 * 100%) * (var(--Grid_Gutter) * 2));
      }
      
      .ft-Footer_Column-social {
      	width: calc((3 / 12 * 100%) * (var(--Grid_Gutter) * 2));
      }

  ## Grid

      .ft-Footer_Columns {
      	grid-template-columns: 5fr repeat(2, 2fr) 3fr;
      
       display: grid;
      }

  This differences between the 2 are pretty obvious and this is a simple example. This also doesn't take in to account any of the responsive code that would have to be written. If that was added then the flex example would probably blow up to about `100` lines of code compared to the Grid example i'll put below because it's so small.

      .ft-Footer_Columns {
      	@media (--sm) {
      		grid-gap: calc(var(--Grid_Gutter) * 2);
      		grid-template-columns: repeat(2, 1fr);
      
      	 display: grid;		
      	}
      
      	@media (--md) {
      		grid-template-columns: repeat(3, 1fr);
      	}
      
      	@media (--lg) {
      		grid-template-columns: 5fr repeat(2, 2fr) 3fr;
      	}
      }
      
      .ft-Footer_Column-content {
      	@media (--md-viewport) {
      		grid-column: 1 / -1;
      	}
      }

  If we took the route of not choosing to use CSS Grid then the 100 line flex example still needs to be written. But we can achieve a lot more power and can iterate over designs a lot quicker with the smaller more terse grid code. The mindset on "support" grid should be that you can get grid support with only a little bit more code than you initially would have written so why would you not?

  ## Benefits of Grid

  You can also achieve layouts with Grid that are very hard to replicate in a dynamic manner with just `flex` or other CSS layout methods.

  ![](https://i.imgur.com/53lUN8l.jpg)

  The above is a good example of this. Theres a few ways this could be written:

  1. Fixed height hero with a position absoluted image.
  1. Statically positioned image with absolutely positioned overlay.
  1. Statically positioned image with statically positioned overlay with a negative margin on the overlay.
  1. Use CSS Grid

  ## 1. Fixed height hero with a position absoluted image.

  The issue i have around this approach is the term `fixed-height` . Most things that have a `fixed` width or height aren't really responsive. Sure we can do a different height at each breakpoint but that's not truly responsive. It also introduces the issue of not knowing what aspect ratio the image can be / should be as we can have it perfect at 1 width and 1 width only when we declare the image must be a certain ratio. Remember back to any issues we've had before when `background-size: cover;` was causing issues for certain images on certain heros. Chances are it was because it was this approach.

  ## 2. Statically positioned image with absolutely positioned overlay.

  I've always been a bit of a believer that most things that are `position: absolute` are a bit of a hack. Obviously there are exceptions to this rule but it's never really a desired approach. Issues we introduce by absolutely positioning the overlay is we need to start introducing "padding hacks" to make sure nothing slips in to the white space below the image. It also introduces how hard it is to absolutely position something on a "grid".

  To get the overlay positioned in the way it is in the image above, we need to have something relative to the `_Inner` to be able to position it to (unless we use Javascript). This introduces a couple more issues, how do we have the image going full width but create another layout that adheres to the `_Inner` we need to position to?

  We'd probably end up with something like:

      <div class="hero">
      	<div class="image"><img></div>
      
      	<div class="content">
      		<div class="content-inner">
      			<div class="overlay">
      			</div>
      		</div>
      	</div>
      </div>

  Remember the image is statically positioned so we get the height we want. Then the `.content` will get pushed to the bottom and we can put the "padding" hack on this to give it the fixed height (uh oh, "fixed" term again) white space below. And we can probably get our desired result albeit a bit hacky and "fixed" in some regards.

  ## 3. Statically positioned image with statically positioned overlay with a negative margin on the overlay.

  Most things above apply for this as well but we achieve it without the absolutely positioning. The main different is now instead of the overlay growing up, it grows down. This creates a big imbalance of white space with the hero and the content below. We also have to have a "fixed" amount of negative `margin-top` again making this not truly responsive.

  ## 4. Use CSS Grid

      <div class="hero">
      	<div class="image"></div>
      	<div class="overlay">
      </div>

      .hero {
      	grid-column-gap: calc(16px * 2);
       grid-template-columns: 1fr minmax(auto, 1290px) 1fr;
       grid-template-rows: 1fr 0.85fr;
      
       display: grid;
      }
      
      .image {
      	grid-column: 1 / -1;
      	grid-row: 1;
      }
      
      .overlay {
      	align-self: end;
      	grid-column: 2;
      	grid-row: 2;
      }

  I feel like the code does most of the talking here. Benefits we get:

  - No "Fixed". Everything is truly fluid / responsive.
  - No hacks
  - Significantly less code
  - Sooo much easier to do responsive with as we have less thing to get in our way

  We've alleviated a lot of the issues we've had with the other 3 methods and arguably (when you understand CSS Grid) it's a lot easier to write and digest.

  Current drawbacks of this method are (i'm not 100% sure on this yet as i haven't done it) i imagine IE11 isn't straightforward to do with this example. But remember i'd quite confidently say over 90% of visitors will have immaculate support for this method. That's not to say it can be ignored, i've had talks with JD about this and it has been said that IE11 doesn't need to look the exact same as Chrome / the designs so there is wiggle room for how easy this can be achieved on IE.
