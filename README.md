from pathlib import Path


class DirectoryTree:

    def __init__(self, rooot):
        self.root = Path(root)
        self.lines = []

    def build_tree(self, folder, prefix=""):
        items = sorted(
            folder.iterdir(),
            key=lambda x: (x.is_file(), x.name.lower())
        )

        for index, item in enumerate(items):
            last = index == len(items) - 1

            branch = "└── " if last else "├── "

            self.lines.append(
                f"{prefix}{branch}{item.name}"
            )

            if item.is_dir():
                extension = "    " if last else "│   "
                self.build_tree(
                    item,
                    prefix + extension
                )

    def generate(self):

        self.lines.append(self.root.name)

        self.build_tree(self.root)

        return "\n".join(self.lines)

    def save(self, filename="tree.txt"):

        content = self.generate()

        with open(
            filename,
            "w",
            encoding="utf-8"
        ) as f:
            f.write(content)

        print(f"Saved to {filename}")


def main():

    folder = input("Folder path: ").strip()

    tree = DirectoryTree(folder)

    print("\nGenerating...\n")

    print(tree.generate())

    tree.save()


if __name__ == "__main__":
    main()
