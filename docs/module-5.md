# Module 5 - Vector Styling & Feature Interaction

Welcome to Module 5. Now that we have vector data (from GeoJSON or WFS) in our map, we need to make it look good and make it interactive.

---

## 1. The Style Object

In OpenLayers, vector styling is controlled by the `Style` class, which is composed of `Fill`, `Stroke`, `Circle` (for points), `Icon`, and `Text`.

```javascript
import {Style, Fill, Stroke, Circle} from 'ol/style.js';

const myStyle = new Style({
  fill: new Fill({
    color: 'rgba(255, 0, 0, 0.5)' // Semi-transparent red
  }),
  stroke: new Stroke({
    color: '#ff0000',
    width: 2
  }),
  image: new Circle({
    radius: 7,
    fill: new Fill({ color: 'blue' })
  })
});

myVectorLayer.setStyle(myStyle);
```

---

## 2. Attribute-Based (Dynamic) Styling

You often want to style features based on their data (e.g., red for high risk, green for low risk). You can pass a **function** instead of a static style object.

```javascript
myVectorLayer.setStyle(function(feature) {
  const riskLevel = feature.get('risk'); // Get the attribute
  let color = 'green';
  
  if (riskLevel === 'high') color = 'red';
  if (riskLevel === 'medium') color = 'orange';

  return new Style({
    fill: new Fill({ color: color })
  });
});
```

---

## 3. Feature Selection & Interaction

To let users click on features, we use the `Select` interaction.

```javascript
import Select from 'ol/interaction/Select.js';
import { click } from 'ol/events/condition.js';

const selectInteraction = new Select({
  condition: click, // Trigger on click
  style: new Style({ // How it looks when selected
    stroke: new Stroke({ color: 'yellow', width: 4 })
  })
});

map.addInteraction(selectInteraction);

// Listen for the select event to read attributes!
selectInteraction.on('select', function(e) {
  if (e.selected.length > 0) {
    const feature = e.selected[0];
    alert("You clicked: " + feature.get('name'));
  }
});
```

---

### Practical activity

**Your Task:**

1. Apply a dynamic style function to your WFS layer. Color the points based on a specific property (e.g., `type`).
2. Add a `Select` interaction to your map.
3. Add an event listener to the select interaction. Instead of an `alert`, `console.log` the entire properties object of the selected feature.