#include <iostream>
using namespace std;

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int key) : val(key), left(nullptr), right(nullptr) {}
};

class BinarySearchTree {
public:
    TreeNode* root;

    BinarySearchTree() : root(nullptr) {}

    TreeNode* search(TreeNode* node, int key) {
        if (node == nullptr || node->val == key)
            return node;

        if (key < node->val)
            return search(node->left, key);
        
        return search(node->right, key);
    }

    TreeNode* insert(TreeNode* node, int key) {
        if (node == nullptr) {
            return new TreeNode(key);
        }

        if (key < node->val)
            node->left = insert(node->left, key);
        else if (key > node->val)
            node->right = insert(node->right, key);

        return node;
    }

    TreeNode* deleteNode(TreeNode* node, int key) {
        if (node == nullptr)
            return node;

        if (key < node->val)
            node->left = deleteNode(node->left, key);
        else if (key > node->val)
            node->right = deleteNode(node->right, key);
        else {
           
            if (node->left == nullptr) {
                TreeNode* temp = node->right;
                delete node;
                return temp;
            } else if (node->right == nullptr) {
                TreeNode* temp = node->left;
                delete node;
                return temp;
            }

            
            TreeNode* temp = minValueNode(node->right);

    
            node->val = temp->val;

            
            node->right = deleteNode(node->right, temp->val);
        }
        return node;
    }

    TreeNode* minValueNode(TreeNode* node) {
        TreeNode* current = node;
        while (current && current->left != nullptr)
            current = current->left;
        return current;
    }

    void inorder(TreeNode* node) {
        if (node != nullptr) {
            inorder(node->left);
            cout << node->val << " ";
            inorder(node->right);
        }
    }
};

int main() {
    BinarySearchTree bst;
    bst.root = bst.insert(bst.root, 50);
    bst.insert(bst.root, 30);
    bst.insert(bst.root, 20);
    bst.insert(bst.root, 40);
    bst.insert(bst.root, 70);
    bst.insert(bst.root, 60);
    bst.insert(bst.root, 80);

    cout << "Inorder traversal of the given tree: ";
    bst.inorder(bst.root);
    cout << endl;

    cout << "Delete 20\n";
    bst.root = bst.deleteNode(bst.root, 20);
    cout << "Inorder traversal after deleting 20: ";
    bst.inorder(bst.root);
    cout << endl;

    cout << "Delete 30\n";
    bst.root = bst.deleteNode(bst.root, 30);
    cout << "Inorder traversal after deleting 30: ";
    bst.inorder(bst.root);
    cout << endl;

    cout << "Delete 50\n";
    bst.root = bst.deleteNode(bst.root, 50);
    cout << "Inorder traversal after deleting 50: ";
    bst.inorder(bst.root);
    cout << endl;

    return 0;
}
