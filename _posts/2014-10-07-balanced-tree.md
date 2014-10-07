---
layout: post
title: "Balance Tree"
description: ""
category: "algorithms"
tags: [tree]
---
{% include JB/setup %}


Balanced Binary Tree

The tree remains balanced as nodes are inserted or deleted

```
private int maxChildHeight(TreeNode<T> node) {
	if(node != null) {
		return 1 + Math.max(maxChildHeight(node.getLeft()), maxChildHeight(node.getRight()));
	}
	return 0;
}

private int getLeftHeight() {
	return maxChildHeight(this.getLeft());
}

private int getRightHeight() {
	return maxChildHeight(this.getRight());
}

private int getBalanceFactor() {
	return  getRightHeight() - getLeftHeight();
}

private TreeState getState() {
	if(this.getLeftHeight() - this.getRightHeight() > 1) {
		return TreeState.LeftHeavy;
	}
	if(this.getRightHeigth() - this.getLeftHeight() > 1) {
		return TreeState.RightHeavy;
	}

	return TreeState.Balanced;
}

```

node rotation

Right Rotation
Left Rotation
Right-Left Rotation
Left-Right Rotation

Right Heavy Tree

if right child is left heavy then left-right rotation
else left rotation

Left Heavy Tree

if left child is right heavy then Right-left rotation
else right rotation

```

private void balance() {
	if(this.getState() == TreeState.RightHeavy) {
		if(this.getRight() != null && this.getRight().getBalanceFactor < 0) {
			leftRightRotation();
		} else {
			leftRotation();
		}
	}
    if(this.getState() == TreeState.LeftHeavy) {
		if(this.getLeft() != null && this.getLeft().getBalanceFactor < 0) {
			rightLeftRotation();
		} else {
			RightRotation();
		}
	}
}

```

Right Rotation

left child becomes the new root
right child of new root is assigned to left child of old root
previous root becomes the new root's right child


Left Rotation

Right child become the new root
left child of new root is assigned to right child of old root
previous root becomes the new root's left child


Right-left rotation

left rotate the left child
right rotate the updated tree


left-right rotation

right rotate the right child
left rotate the updated tree
