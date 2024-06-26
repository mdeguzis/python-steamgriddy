#!/bin/python3

import os
import json

from steamgriddy import SteamGridDB
from steamgriddy import steam_helpers
from steamgriddy import enums

# TESTING ONLY
border = '=' * 80
try:
    sgdb = SteamGridDB(os.environ["STEAMGRID_API_KEY"])
except KeyError:
    exit("Please set your Steam API key to the env var 'STEAMGRID_API_KEY'")

# Get games in users library to interate through
users = steam_helpers.get_steam_users()
if len(users) > 1:
    exit("Multiple Steam users found on this system, please set --steam-user <NAME>")

exit

# By game ID
print(f"\n{border}")
game_data = sgdb.get_game_by_steam_appid(207650)
game_data_json = game_data.to_json()
print(game_data_json)

# Get games in users library to interate through
users = steam_helpers.get_steam_users()
if len(users) > 1:
    exit("Multiple Steam users found on this system, please set --steam-user <NAME>")

print(users)

# Get library folders
print("Trying to get library folders from VDF")
if not steam_helpers.get_library_folders():
    exit("Failed to find library folders!")
print(steam_helpers.get_library_folders())

# Parse usr games from profile
# Skip this for testing unless needed to reduce calls
#users = steam_helpers.get_steam_users()
#for user in users:
#    user_id = user['username']
#    steam_id = user['steam_id']
#    print(steam_helpers.fetch_and_parse_games_xml(steam_id))

# SteamGridDB API V2 docs: https://www.steamgriddb.com/api/v2
print("Testing grid stuff")
grids = []
grids = sgdb.get_grids_by_gameid([game_data_json["id"]])
grids = sgdb.get_grids_by_gameid(game_ids = [game_data_json["id"]], dimensions = ["600x900"])
for grid in grids:
    print(json.dumps(grid.to_json(), indent=4))

print("DONE!")
