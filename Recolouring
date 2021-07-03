import os
import cv2
import torch
import glob
import argparse
import numpy as np
from PIL import Image
from tqdm import tqdm

def get_arguments():
    """Parse all the arguments provided from the CLI.
    Returns:
      A list of parsed arguments.
    """
    parser = argparse.ArgumentParser(description="Self Correction for Human Parsing")

    parser.add_argument("--input_path", type=str)
    parser.add_argument("--parsed_output_path", type=str)
    parser.add_argument("--recoloured_output_folder", type=str)

    return parser.parse_args()


def cloth_colour(inp,out):        # inp=input image , out=parsed image
    top_range1 = np.array([128,128,128], dtype = "uint8")
    top_range2 = np.array([128,0,0], dtype = "uint8")
    top_range3 = np.array([128,0,128], dtype = "uint8")
    top_mask = (cv2.inRange(out,top_range1,top_range1)) | (cv2.inRange(out,top_range2,top_range2)) | (cv2.inRange(out,top_range3,top_range3))
    #top_mask = cv2.inRange(out,top_range1,top_range1)
    top_mask = cv2.cvtColor(top_mask, cv2.COLOR_BGR2RGB)
    bot_range = np.array([128,128,0], dtype = "uint8")
    bot_mask = cv2.inRange(out,bot_range,bot_range)
    bot_mask = cv2.cvtColor(bot_mask, cv2.COLOR_BGR2RGB)
    bg_range = np.array([0,0,0], dtype = "uint8")
    bg_mask = cv2.inRange(out,bg_range,bg_range)
    bg_mask = cv2.cvtColor(bg_mask, cv2.COLOR_BGR2RGB)
    for i in range(inp.shape[0]):
        for j in range(inp.shape[1]):
            if (top_mask[i,j]==np.array([255,255,255])).all():
                inp[i,j]=[255,144,30]
            if (bot_mask[i,j]==np.array([255,255,255])).all():
                inp[i,j]=[0,255,255]
            if (bg_mask[i,j]==np.array([255,255,255])).all():
                inp[i,j]=[255,255,255]

def main():
    args = get_arguments()
    path1 = args.input_path
    path2 = args.parsed_output_path
    path3 = args.recoloured_output_folder

    if not os.path.exists(args.recoloured_output_folder):
        os.makedirs(args.recoloured_output_folder)

    for x, y in zip(sorted(glob.glob(path1)),sorted(glob.glob(path2))):
        inp = cv2.imread(x)
        out = cv2.imread(y)
        cloth_colour(inp,out)
        cv2.imwrite(path3+x.split("/")[-1],inp)

    return

if __name__ == '__main__':
    main()
