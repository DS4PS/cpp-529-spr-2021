---
layout: default
---

<div class = "uk-container uk-container-small">
  
<br><br>

# Creating Color Scales for Choropleth Maps

```r
x <- rnorm( 10000, 250000, 50000 )

# equally spaced bins
hist( x/100000, breaks=25, col="gray", border="white" )
abline( v=cut.points/100000, col="darkred", lwd=2 )

# lead to clumpy data
#   may be misleading 
barplot( table( cut( x, breaks=10 ) ) )

# quantile function identifies values at specific percentiles 
quantile( x, c(0, 0.25, 0.5, 0.75, 1) )
summary( x )



# create equal data bins - deciles here
cut.points <- quantile( x, seq(0,1,0.1) )
y <- cut( x, breaks=cut.points, labels=paste0("G",1:10) )
table( y )

# shortcut - use rank of x instead of x
barplot( table( cut( rank( x ), breaks=10 ) )  )



# build your choropleth color ramp:
# sequential or divergent

# colorRampPalette creates a color generating function 

color.function <- colorRampPalette( c("gray80","darkred") )
col.ramp <- color.function( 7 ) # number of groups you desire
points( 1:7, rep(3,7), pch=15, cex=8, col=col.ramp )

color.function <- colorRampPalette( c("darkred","gray80","steelblue") )
col.ramp <- color.function( 7 ) # number of groups you desire
points( 1:7, rep(2,7), pch=15, cex=8, col=col.ramp )

color.function <- colorRampPalette( c("gray80","black") )
col.ramp <- color.function( 7 ) # number of groups you desire
points( 1:7, rep(1,7), pch=15, cex=8, col=col.ramp )

text( 8, 3, "Sequential", pos=4 )
text( 8, 2, "Divergent", pos=4 )
text( 8, 1, "Grayscale", pos=4 )


# view your color scale: 
color.function <- colorRampPalette( c("darkred","gray80","steelblue") )
col.ramp <- color.function( 10 )
barplot( table( cut( x, breaks=cut.points ) ), col=col.ramp  )


# create a vector of color labels for your data
cut.points <- quantile( x, seq(0,1,0.1) )
f <- cut( x, breaks=cut.points, labels=col.ramp )
head( data.frame( x, f ) )

# add to your map using the color vector
# assuming x represents attribute of tract here: 
plot( sp.map, col=f )

```



<br><br>

</div>
