#!/bin/bash

for ((;;))do
  read -p "Do you want to continue? [y/n]: " answer
  if [ "$answer" = "y" ]; then
    echo "You chose to continue."
    break
  fi
done

# do something