```
function getTitles() {
    // Create base XPath pattern
    const baseXPath = "/html/body/main/div[3]/article/div/div[5]/div[1]/div/div/div[2]/div[2]/ol/li";
    const titles = [];
    
    // Function to get element by XPath
    function getElementByXPath(xpath) {
        return document.evaluate(
            xpath,
            document,
            null,
            XPathResult.FIRST_ORDERED_NODE_TYPE,
            null
        ).singleNodeValue;
    }

    // Loop through list items (assuming they're sequential from 81 to 82)
    for (let i = 1; i <= 109; i++) {
        const fullXPath = `${baseXPath}[${i}]/div[2]/div[1]/div/div[1]`;
        const element = getElementByXPath(fullXPath);
        
        if (element) {
            titles.push(element.textContent.trim());
        }
    }

    // Log the results
    console.log("Found titles:", titles);
    return titles;
}

// Execute the function
getTitles();
```
获取到的是['Massachusetts Institute of Technology', 'Stanford University', ...]列表，点击尖头展开后，**成了排列**
1. 0: "Massachusetts Institute of Technology麻省理工学院"
2. 1: "Stanford University斯坦福大学"
3. 2: "University of California, Berkeley加利福尼亚大学伯克利分校"
4. 3: "California Institute of Technology加州理工学院"