# Grasshopper Sketches

## Step Surface
[Download Definition](https://github.com/borolgs/gh-sketches/raw/master/gh/step-surface.gh)  


<details>
<summary><a href="https://github.com/borolgs/gh-sketches/raw/master/gh/step-surface.dyn">Dynamo Version</a> </summary>
<p>
  
```javascript
u = 4;
v = 12;

//Divide Surface
uRange = (0..1..#u);
vRange = (0..1..#v);
pts = Surface.PointAtParameter(srf<1>, uRange<2>, vRange<3>);

//Make Steps
newPts = makeSteps(pts);

// Create polyCurves
polyCrvs = PolyCurve.ByPoints(
	Point.PruneDuplicates(newPts, 0.001),
	false
);

// New Surface
stepSrf = Surface.ByLoft(polyCrvs);

def makeSteps(pts: Point[]) {
	return [Imperative] {
		resultPts = [];
		count = List.Count(pts) - 1;
		for(i in 0..count) {
			old2Pt = pts[i+1 > count ? i : i + 1];
			new2Pt = Point.ByCoordinates(old2Pt.X, old2Pt.Y, pts[i].Z);
			resultPts = List.Join([resultPts, [pts[i], new2Pt]]);
		}
		return resultPts;
	};
};
```

</p>
    
</details>
<p>
  <img src="/docs/images/step-surface.jpg" width="300" />
  <img src="/docs/images/step-surface.png" width="500" />
</p>
