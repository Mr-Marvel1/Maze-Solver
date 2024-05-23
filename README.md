# Maze-Solver => The maze is represented as a 2D vector, where 1 represents walls and 0 represents open paths. The solver will find a path from the start position to the end position.

#include <iostream>
#include <vector>
#include <bits/stdc++.h>
using namespace std;

bool isSafe(const vector<vector<int>>& maze, int x, int y, const vector<vector<bool>>& visited) {
    return (x >= 0 && x < maze.size() && y >= 0 && y < maze[0].size() && maze[x][y] == 0 && !visited[x][y]);
}

bool solveMazeDFS(const vector<vector<int>>& maze, int x, int y, int endX, int endY, vector<pair<int, int>>& path, vector<vector<bool>>& visited) {
    
    if (x == endX && y == endY) {
        path.push_back({x, y});
        return true;
    }

    if (isSafe(maze, x, y, visited)) {
        visited[x][y] = true;
        path.push_back({x, y});

        if (solveMazeDFS(maze, x, y + 1, endX, endY, path, visited)) {
            return true;
        }
       
        if (solveMazeDFS(maze, x + 1, y, endX, endY, path, visited)) {
            return true;
        }
       
        if (solveMazeDFS(maze, x, y - 1, endX, endY, path, visited)) {
            return true;
        }
       
        if (solveMazeDFS(maze, x - 1, y, endX, endY, path, visited)) {
            return true;
        }

        path.pop_back();
        visited[x][y] = false;
    }

    return false;
}

vector<pair<int, int>> findPath(const vector<vector<int>>& maze, int startX, int startY, int endX, int endY) {
    vector<pair<int, int>> path;
    vector<vector<bool>> visited(maze.size(), vector<bool>(maze[0].size(), false));

    if (solveMazeDFS(maze, startX, startY, endX, endY, path, visited)) {
        return path;
    } else {
        return {}; 
    }
}

int main() {
    vector<vector<int>> maze = {
        {0, 1, 0, 0, 0},
        {0, 1, 0, 1, 0},
        {0, 0, 0, 1, 0},
        {0, 1, 1, 1, 0},
        {0, 0, 0, 0, 0}
    };

    int startX = 0, startY = 0;
    int endX = 4, endY = 4;

    vector<pair<int, int>> path = findPath(maze, startX, startY, endX, endY);

    if (!path.empty()) {
        cout << "Path found:" << endl;
        for (const auto& pos : path) {
            cout << "(" << pos.first << ", " << pos.second << ") ";
        }
        cout << endl;
    } else {
        cout << "No path found." << endl;
    }

    return 0;
}
