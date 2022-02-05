# Unofficial PsychoMollies Rank Table

This is an unofficial ranking table of the PsychoMollies NFT collection. It is based on my own calculations and does not claim to be valid. I fetched the data from https://crypto.com/nft-api/graphql

# Score Calculation

The score, by which the mollies are sorted, is obtained by multiplying their traits. To simplify the sorting of the score, I normalized the score to the interval [0,1], where values closer to 0 are very rare and values closer to 1 are less rare. 

```python
def getTraitsScore(attributes):
    score = 1
    for i in range(0,len(attributes)):
        p = attributes.loc[i]["percentage"]
        score *= float(p)
    return score
```
```python
def normalize(old_value,mn,mx):
    return (old_value - mn) / (mx - mn)
```
```python
def appendScoreColumn(mollies):
	mollies["score"] = mollies.apply(lambda x: getTraitsScore(x.attributes),axis=1)
	mollies["score"] = mollies["score"].apply(lambda x: normalize(x))
	result = mollies.sort_values(by=['score'], ascending=True)
	result = result.reset_index(drop=True)
	return result
```